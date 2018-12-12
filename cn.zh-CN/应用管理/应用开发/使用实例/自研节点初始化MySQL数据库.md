# 自研节点初始化MySQL数据库 {#concept_uzx_gnv_1gb .concept}

本章介绍如何使用自研镜像初始化第三方MySQL数据库和RDS for MySQL数据库。

## 操作步骤 {#section_m45_zpv_1gb .section}

1.  创建用于数据库初始化的sql文件，在文件中写入数据库表的创建等相关初始化脚本，可以从现有的数据库中直接导出初始化sql脚本文件来使用。脚本示例如下：

    ```
    /******************************************/
    /*   创建user数据表，并插入两条初始化数据       */
    /******************************************/
    CREATE TABLE IF NOT EXISTS `user` (
     `id` bigint(20) unsigned NOT NULL AUTO_INCREMENT COMMENT '主键',
     `gmt_create` datetime NOT NULL COMMENT '创建时间',
     `gmt_modified` datetime NOT NULL COMMENT '修改时间',
     `name` varchar(256) NOT NULL COMMENT 'name',
     `phone` varchar(64) NOT NULL DEFAULT '' COMMENT 'phone',
     PRIMARY KEY (`id`)
     ) ENGINE=InnoDB AUTO_INCREMENT=4 DEFAULT CHARSET=utf8 COLLATE utf8_general_ci COMMENT='用户表';
    
    INSERT INTO `user` (id, gmt_create, gmt_modified, name, phone) VALUES ('1', '2018-11-26 10:00:00', '2018-11-26 10:00:00', '测试', '1388888888');
    INSERT INTO `user` (id, gmt_create, gmt_modified, name, phone) VALUES ('2', '2018-11-26 10:00:00', '2018-11-26 10:00:00', 'test', '1366666666');
    ```

    **说明：** 

    -   如果使用从现有库导出的sql文件，请务必修改建表语句。

        删除`DROP TABLE IF EXISTS 'tableName'`，并对建表语句加上`IF NOT EXISTS`限定词，防止镜像重启将数据库表重新初始化。

    -   为了更好地支持中文，字符集建议使用UTF-8。
2.  创建用于容器初始化的运行脚本，在脚本中添加MySQL调用数据库表初始化sql文件的脚本指令，以及启动自有应用的指令（本文档以iot-demo.jar自有应用为例）。

    对于原生MySQL和RDS MySQL节点，容器中存放访问信息的环境变量不同，因此如下示例脚本分别演示了MySQL和RDS MySQL节点的配置方法。

    **说明：** 

    -   在脚本头部需要使用`#!/bin/bash --login`注明脚本加载模式，确保容器内的环境变量可以被应用正常获取。
    -   在编写MySQL执行指令脚本时，需要注意`-p`与密码信息之间不能有空格，具体以本文下方MySQL节点的配置示例脚本为准。
    -   启动`jar`应用时建议加上内存限制，防止内存使用超过容器限制而无法正常启动容器。
    MySQL节点的配置示例：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #MySQL
    USER=$(prop 'iot.hosting.{这里填写服务名称}.mysqlUser')
    PASSWORD=$(prop 'iot.hosting.{这里填写服务名称}.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot.hosting.{这里填写服务名称}.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    ######################
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -D ${DBNAME} < db.sql
    java -jar -Xms512m -Xmx512m /iot-demo.jar --server.port=8080
    ```

    脚本中MySQL访问信息对应的环境变量中动态内容，需要与应用配置中部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78131/154458217933865_zh-CN.png)

    RDS MySQL节点的配置示例：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #RDS MySQL
    USER=$(prop 'iot.hosting.rds.{这里填写RDS名称}.mysqlUser')
    PASSWORD=$(prop 'iot.hosting.rds.{这里填写RDS名称}.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot.hosting.rds.{这里填写RDS名称}.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    ######################
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -D ${DBNAME} < db.sql
    java -jar -Xms512m -Xmx512m /iot-demo.jar --server.port=8080
    ```

    脚本中RDS MySQL访问信息对应的环境变量中动态内容，需要与应用配置中部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78131/154458217933866_zh-CN.png)

3.  编写Dockerfile文件，将编写好的sh文件（容器初始化运行脚本）和sql文件（数据库表初始化sql文件）拷贝到容器指定的目录，并配置相应的执行权限。

    为了能够通过脚本访问MySQL数据库，需要同时配置安装MySQL-Client，示例如下：

    ```
    FROM maven:3.5-jdk-8
    COPY target/iot-demo-0.0.1-SNAPSHOT.jar /iot-demo.jar
    COPY db.sql /db.sql
    COPY init.sh /init.sh
    RUN chmod 777 /db.sql
    RUN chmod 777 /init.sh
    RUN chmod +x /init.sh
    RUN apt-get update && apt-get -y install openjdk-8-jdk && apt-get -y install mysql-client
    EXPOSE 8080
    # 设置系统的字符集，注意不同的基础镜像安装的字符集有差异，需要自行调整
    ENV LANG=C.UTF-8
    ENTRYPOINT ["/bin/bash","-c","/init.sh"]
    ```

4.  使用docker build命令进行镜像构建，完成后推送到应用托管镜像仓库即可进行配置部署。

