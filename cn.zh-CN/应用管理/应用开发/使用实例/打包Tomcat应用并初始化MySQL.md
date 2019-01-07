# 打包Tomcat应用并初始化MySQL {#concept_vfq_wbm_cgb .concept}

本章介绍如何使用自研镜像初始化第三方MySQL数据库并打包Tomcat应用。

## 准备工作 {#section_thj_1nn_cgb .section}

1.  准备数据库初始化文件。

    创建用于数据库初始化的sql文件，在文件中写入数据库表的创建等相关初始化脚本，可以从现有的数据库中直接导出初始化sql脚本文件来使用。脚本示例如下：

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
    -   部署多个自研节点副本的时候，防止insert插入出现主键和uniqueKey重复。
    如果表名需要忽略大小写（默认MySQL表名大小写敏感），需要把如下所示的MySQL cnf配置拷贝到MySQL配置节点的配置数据库cnf中。

    MySQL cnf配置：

    ```
    [mysqld]
    skip-name-resolve
    max_connections = 1000
    query_cache_size = 0
    innodb_buffer_pool_size = 256M
    innodb_log_file_size = 512M
    innodb_flush_log_at_trx_commit = 1
    innodb_flush_method = O_DIRECT
    log-bin = mysql-bin
    server-id = 1
    character-set-server = utf8
    collation-server = utf8_general_ci
    lower_case_table_names=1
    [mysql]
    default-character-set = utf8
    ```

    配置数据库cnf：

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79832/154685368734220_zh-CN.png)

2.  获取Tomcat的压缩包。

    从[Tomcat官网](https://tomcat.apache.org/download-80.cgi#8.5.35)下载对应版本的.tar压缩包（本文示例使用8.5.35版本）。


## 操作步骤 {#section_ir4_rqn_cgb .section}

1.  （可选）创建用于修改Tomcat默认首页用的跳转页面文件。

    **说明：** 若希望访问应用URL时直接进入应用自身的首页，而非Tomcat的默认首页，那么需要创建用于修改Tomcat默认首页用的跳转页面文件（index.jsp文件）。

    创建index.jsp文件，在文件中输入以下内容并保存。

    ```
    <html>
    <script type="text/javascript">
       top.location='demo';
    </script>
    <html>
    ```

    其中，`top.location='demo'`中的`demo`需要根据实际情况替换为自有的web应用.war包名称，本文档使用demo.war测试应用做示例，因此此处取值为`demo`。

2.  创建用于容器初始化的运行脚本，在脚本中添加sqlcmd调用数据库初始化sql文件的脚本指令，以及启动Tomcat的指令。

    对于原生MySQL和RDS MySQL节点，容器中存放访问信息的环境变量不同，因此如下示例脚本分别演示了MySQL和RDS MySQL节点的配置方法。

    **说明：** 

    -   在脚本头部需要使用`#!/bin/bash --login`注明脚本加载模式，确保容器内的环境变量可以被应用正常获取。
    -   在编写MySQL执行指令脚本时，需要注意`-p`与密码信息之间不能有空格，具体以本文下方MySQL节点的配置示例脚本为准。
    MySQL节点的配置示例：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #从容器的环境变量中获取MySQL访问信息
    USER=$(prop 'iot\.hosting\.{服务名称}\.mysqlUser')
    PASSWORD=$(prop 'iot\.hosting\.{服务名称}\.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot\.hosting\.{服务名称}\.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    #通过sql文件初始化数据库
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -D ${DBNAME} < db.sql
    
    #启动tomcat
    /usr/local/apache-tomcat-8.5.35/bin/catalina.sh run
    ```

    其中，示例中的\{服务名称\}，需替换为实际服务名称，例如服务名称为xxx，则实际配置代码如下：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #从容器的环境变量中获取MySQL访问信息
    USER=$(prop 'iot\.hosting\.xxx\.mysqlUser')
    PASSWORD=$(prop 'iot\.hosting\.xxx\.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot\.hosting\.xxx\.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    #通过sql文件初始化数据库
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -f ${DBNAME} < db.sql
    
    #启动tomcat
    /usr/local/apache-tomcat-8.5.35/bin/catalina.sh run
    ```

    脚本中MySQL访问信息对应的环境变量中动态内容，需要与应用配置中部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79832/154685368734224_zh-CN.png)

    RDS MySQL节点的配置示例：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #从容器的环境变量中获取RDS MySQL访问信息
    USER=$(prop 'iot.hosting.rds.{RDS名称}.mysqlUser')
    PASSWORD=$(prop 'iot.hosting.rds.{RDS名称}.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot.hosting.rds.{RDS名称}.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    #通过sql文件初始化数据库
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -D ${DBNAME} < db.sql
    
    #启动运行自有应用
    /usr/local/apache-tomcat-8.5.35/bin/catalina.sh run
    ```

    其中，示例中的\{RDS名称\}，需替换为实际RDS名称，例如RDS名称为xxx，则实际配置代码如下：

    ```
    #!/bin/bash --login
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #从容器的环境变量中获取RDS MySQL访问信息
    USER=$(prop 'iot.hosting.rds.xxx.mysqlUser')
    PASSWORD=$(prop 'iot.hosting.rds.xxx.mysqlPassword')
    HOSTNAME_FULL=$(prop 'iot.hosting.rds.xxx.mysqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:mysql://}
    HOSTNAME=${HOSTNAME_TEMP%:*}
    DBNAME=${HOSTNAME_TEMP##*/}
    
    #通过sql文件初始化数据库
    mysql -h ${HOSTNAME} -u ${USER} -p${PASSWORD} -D ${DBNAME} < db.sql
    
    #启动运行自有应用
    /usr/local/apache-tomcat-8.5.35/bin/catalina.sh run
    ```

    脚本中RDS MySQL访问信息对应的环境变量中动态内容，需要与应用配置中部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79832/154685368734225_zh-CN.png)

3.  编写Dockerfile文件，将编写好的sh文件（容器初始化运行脚本）和sql文件（数据库表初始化sql文件）拷贝到容器指定的目录，并配置相应的执行权限。

    为了能够通过shell脚本访问MySQL数据库，需要同时配置安装MySQL-Client，示例如下（示例中的demo.war为测试应用，实际使用时替换成自有的web应用）：

    ```
    FROM ubuntu:16.04
    
    # 安装jdk8
    RUN apt-get update && apt-get install -y openjdk-8-jdk
    
    # 安装tomcat
    ADD apache-tomcat-8.5.35.tar.gz /usr/local
    ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.35
    ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.35
    ENV PATH $PATH:$CATALINA_HOME/lib:$CATALINA_HOME/bin
    
    # 安装MySQL Client环境
    RUN apt-get update && apt-get -y install mysql-client
    
    # 安装中文显示环境,在终端操作时，确保能正确的显示中文内容
    RUN apt-get update && apt-get install -y locales
    ENV LANG C.UTF-8
    
    # 复制自有app应用
    COPY demo.war /usr/local/apache-tomcat-8.5.35/webapps/
    # 修改tomcat的启动页为自有项目的启动页
    COPY index.jsp /usr/local/apache-tomcat-8.5.35/webapps/ROOT/
    # 复制数据库sql和初始化脚本
    COPY db.sql /db.sql
    COPY init.sh /init.sh
    RUN chmod +x /init.sh
    
    EXPOSE 8080
    ENTRYPOINT ["/bin/bash","-c","/init.sh"]
    ```

4.  将相关的资源文件放置在同一个目录中，并使用docker build命令进行镜像构建，完成后推送到应用托管镜像仓库即可进行配置部署。

