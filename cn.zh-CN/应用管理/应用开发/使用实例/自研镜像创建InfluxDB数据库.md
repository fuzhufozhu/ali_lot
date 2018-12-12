# 自研镜像创建InfluxDB数据库 {#concept_jyb_jnv_1gb .concept}

由于InfluxDB节点部署完成之后并不能立即使用，需要手动创建数据库，方可使用。因此，本文演示了如何利用自研镜像，在其初始化过程中对InfluxDB节点进行初始化。

## 操作步骤 {#section_apj_myv_1gb .section}

1.  创建用于容器初始化的运行脚本，在脚本中添加`influxdb`创建数据库的命令，示例如下所示。

    **说明：** 在脚本头部需要使用`#!/bin/bash --login`注明脚本加载模式，确保容器内的环境变量可以被应用正常获取。

    ```
    #!/bin/bash --login
    USER="{这里填写配置的数据库访问用户名}"
    PASSWORD="{这里填写配置的数据库访问密码}"
    SERVICENAME="{这里填写配置的数据库访问服务名}"
    DBNAME="{这里填写配置的数据库名称}"
    
    curl -XPOST "http://${SERVICENAME}:8086/query?u=${USER}&p=${PASSWORD}" --data-urlencode "q=CREATE DATABASE \"${DBNAME}\""
    ```

    `Influxdb`的访问信息需要与应用配置中`Influxdb`部署节点的配置保持一致。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/78132/154458219233872_zh-CN.png)

2.  编写Dockerfile文件。

    **说明：** 基础镜像根据业务需要自行设置。

    ```
    FROM maven:3.5-jdk-8
    COPY init.sh /init.sh
    RUN chmod 777 /init.sh
    RUN chmod +x /init.sh
    # 设置系统的字符集，注意不同的基础镜像安装的字符集有差异，需要自行调整
    ENV LANG=C.UTF-8
    ENTRYPOINT ["/bin/bash","-c","/init.sh"]
    ```

3.  使用docker build命令进行镜像构建，完成后推送到应用托管镜像仓库即可进行配置部署。

