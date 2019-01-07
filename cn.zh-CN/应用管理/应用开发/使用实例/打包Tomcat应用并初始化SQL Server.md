# 打包Tomcat应用并初始化SQL Server {#concept_y3g_ybm_cgb .concept}

本章介绍如何使用自研镜像初始化SQL Server数据库并打包Tomcat应用。

## 准备工作 {#section_thj_1nn_cgb .section}

1.  准备数据库初始化文件。

    创建用于数据库初始化的sql文件，在文件中写入数据库表的创建等相关初始化脚本，可以从现有的数据库中直接导出初始化sql脚本文件来使用。脚本示例如下：

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

    **说明：** 

    -   如果使用现有库导出的sql文件，请务必确保sql文件中已经添加了创建数据库的相关语句。
    -   部署多个自研节点副本的时候，防止insert插入出现主键和uniqueKey重复。
    -   导出的sql文件请不要携带本地系统的数据库文件的配置信息。
    -   字符串属性字段请使用`nvarchar`，防止中文字符出现乱码，同时在脚本中插入中文字段时，需要在中文字段前添加英文字符`N`标识，如上述示例所示。
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

    **说明：** 在脚本头部需要使用`#!/bin/bash --login`注明脚本加载模式，确保容器内的环境变量可以被应用正常获取。

    ```
    #!/bin/bash --login
    
    function prop() {
        env|grep "${1}"|cut -d'=' -f2
    }
    
    #从容器中的环境变量获取SQLServer访问参数
    USER=$(prop 'iot\.hosting\.{这里填写服务名称}\.mssqlUser')
    PASSWORD=$(prop 'iot\.hosting\.{这里填写服务名称}\.mssqlPassword')
    HOSTNAME_FULL=$(prop 'iot\.hosting\.{这里填写服务名称}\.mssqlUrl')
    HOSTNAME_TEMP=${HOSTNAME_FULL#jdbc:sqlserver://}
    HOSTNAME=${HOSTNAME_TEMP%:*}",1433"
    
    #使用数据库初始化sql文件初始化数据库
    /opt/mssql-tools/bin/sqlcmd -S ${HOSTNAME} -U ${USER} -P ${PASSWORD} -i db.sql
    
    #启动tomcat
    /usr/local/apache-tomcat-8.5.35/bin/catalina.sh run
    ```

    脚本中SQL Server访问信息对应的环境变量中的动态部分内容，需要与应用配置中的对应部署节点的设置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79833/154685369934258_zh-CN.png)

3.  编写Dockerfile文件，将编写好的sh文件（容器初始化运行脚本）和sql文件（数据库表初始化sql文件）拷贝到容器指定的目录，并配置相应的执行权限。

    同时需要配置sqlcmd（用于shell脚本访问SQL Server数据库的应用）的安装指令以及tomcat的安装指令。示例如下（示例中的demo.war为测试应用，实际使用时替换成自有的web应用）：

    **说明：** 当前sqlcmd必须使用Ubuntu 16.04版本作为基础镜像来源才能被正确安装。

    ```
    FROM ubuntu:16.04
    
    # 安装jdk8
    RUN apt-get update && apt-get install -y openjdk-8-jdk
    
    # 安装tomcat
    ADD apache-tomcat-8.5.35.tar.gz /usr/local
    ENV CATALINA_HOME /usr/local/apache-tomcat-8.5.35
    ENV CATALINA_BASE /usr/local/apache-tomcat-8.5.35
    ENV PATH $PATH:$CATALINA_HOME/lib:$CATALINA_HOME/bin
    
    # 安装sqlcmd环境
    RUN apt-get update && apt-get install -y \
    	curl apt-transport-https debconf-utils \
        && rm -rf /var/lib/apt/lists/*
    RUN curl https://packages.microsoft.com/keys/microsoft.asc | apt-key add -
    RUN curl https://packages.microsoft.com/config/ubuntu/16.04/prod.list > /etc/apt/sources.list.d/mssql-release.list
    RUN apt-get update && ACCEPT_EULA=Y apt-get install -y msodbcsql mssql-tools
    RUN echo 'export PATH="$PATH:/opt/mssql-tools/bin"' >> ~/.bashrc
    RUN /bin/bash -c "source ~/.bashrc"
    
    # 安装中文显示环境,在终端操作时，确保能正确的显示中文内容
    RUN apt-get update && apt-get install -y locales
    ENV LANG C.UTF-8
    
    # 因sqlcmd限制，需要配置终端环境使用en_US.UTF-8
    RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen
    
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

