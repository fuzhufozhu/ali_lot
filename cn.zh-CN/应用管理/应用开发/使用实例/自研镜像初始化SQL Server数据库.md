# 自研镜像初始化SQL Server数据库 {#task_bpl_2lx_bgb .task}

本章介绍如何使用自研镜像初始化SQL Server数据库。

本文描述的自研镜像初始化SQL Server数据库，是针对SQL Server镜像有效的，暂不支持RDS版本。

1.  创建用于数据库初始化的sql文件，在文件中写入数据库表的创建等相关初始化脚本，可以从现有的数据库中直接导出初始化sql脚本文件来使用。脚本示例如下： 

    ```
    /******************************************/
    /*   创建user数据表，并插入两条初始化数据       */
    /******************************************/
    CREATE DATABASE demoDB
    GO
    CREATE TABLE [demoDB].[dbo].[user]
    (
    	[id] bigint NOT NULL ,
    	[gmt_create] datetime NOT NULL ,
    	[gmt_modified] datetime NOT NULL ,
    	[name] varchar(50) NOT NULL ,
    	[phone] varchar(50) NOT NULL 
    )
    ON [PRIMARY]
    GO
    ALTER TABLE [demoDB].[dbo].[user] ADD CONSTRAINT [PK_user] PRIMARY KEY CLUSTERED 
    (
    [id]
    )
    WITH( STATISTICS_NORECOMPUTE = OFF, IGNORE_DUP_KEY = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)
    GO
    INSERT INTO [demoDB].[dbo].[user] (id, gmt_create, gmt_modified, name, phone) VALUES ('1', '2018-11-26 10:00:00', '2018-11-26 10:00:00', '测试', '1388888888');
    INSERT INTO [demoDB].[dbo].[user] (id, gmt_create, gmt_modified, name, phone) VALUES ('2', '2018-11-26 10:00:00', '2018-11-26 10:00:00', 'test', '1366666666');
    GO
    ```

    **说明：** 如果使用现有库导出的sql文件，请务必确保sql文件中已经添加了创建数据库的相关语句。

2.  创建用于容器初始化的运行脚本，在脚本中添加sqlcmd调用数据库初始化sql文件的脚本指令，以及启动自有应用的指令（本文档以iot-demo.jar自有应用为例）。 

    **说明：** 

    -   在脚本头部需要使用`#!/bin/bash --login`注明脚本加载模式，确保容器内的环境变量可以被应用正常获取。
    -   启动`jar`应用时建议加上内存限制，防止内存使用超过容器限制而无法正常启动容器。
    ```
    #!/bin/bash --login
    
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #mssql
    USER=$(prop 'iot.hosting.{这里填写服务名称}.mssqlUser')
    PASSWORD=$(prop 'iot.hosting.{这里填写服务名称}.mssqlPassword')
    HOSTNAME_FULL=$(prop 'iot.hosting.{这里填写服务名称}.mssqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:sqlserver://}
    HOSTNAME=${HOSTNAME_TEMP%:*}",1433"
    
    ######################
    
    /opt/mssql-tools/bin/sqlcmd -S ${HOSTNAME} -U ${USER} -P ${PASSWORD} -i db.sql
    java -jar -Xms512m -Xmx512m /iot-demo.jar --server.port=8080
    ```

    脚本中SQL Server访问信息对应的环境变量中动态内容，需要与应用配置中部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78711/154461388034066_zh-CN.png)

3.  编写Dockerfile文件，将编写好的sh文件（容器初始化运行脚本）和sql文件（数据库表初始化sql文件）拷贝到容器指定的目录，并配置相应的执行权限。 

    同时需要配置sqlcmd（用于shell脚本访问SQL Server数据库的应用）的安装指令以及tomcat的安装指令，建议您使用Ubuntu作为基础镜像来源。示例如下：

    **说明：** 当前sqlcmd必须使用Ubuntu 16.04版本作为基础镜像来源才能被正确安装。

    ```
    FROM ubuntu:16.04
    
    # 安装sqlcmd环境
    RUN apt-get update && apt-get install -y \
    	curl apt-transport-https debconf-utils \
        && rm -rf /var/lib/apt/lists/*
    RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
    RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
    RUN apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools
    RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
    RUN /bin/bash -c "source ~/.bashrc"
    RUN apt-get install -y locales
    RUN apt-get update && apt-get -y install openjdk-8-jdk
    RUN locale-gen en_US.UTF-8
    RUN update-locale LANG=en_US.UTF-8
    
    # 复制自有应用
    COPY iot-demo-0.0.1-SNAPSHOT.jar /iot-demo.jar
    
    # 复制数据库初始化文件和启动脚本
    COPY db.sql /db.sql
    COPY init.sh /init.sh
    RUN chmod 777 /db.sql
    RUN chmod 777 /init.sh
    RUN chmod +x /init.sh
    
    EXPOSE 8080
    ENTRYPOINT ["/bin/bash","-c","/init.sh"]
    ```

4.  使用docker build命令进行镜像构建，完成后推送到应用托管镜像仓库即可进行配置部署。 

