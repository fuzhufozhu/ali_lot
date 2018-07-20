# 基于Topic路由服务的M2M设备间通信 {#task_q45_glr_ydb .task}

用户控制智能灯的流程如[图 1](#fig_tdr_mlr_ydb)所示。

![](images/4210_zh-CN.png "流程图")

智能灯采用MQTT长连接接入，申请的ProductKey为pk1，deviceName为light。

1.   手机App用户使用自己的server注册登录，用户server将从物联网平台中申请设备，并返回给手机App，让手机App可以作为一个设备连接到物联网平台。 

    图中是HTTPS接入，申请的deviceName为controlApp。

2.   将用户与智能灯绑定，当App请求用户server，server返回绑定成功的deviceName，即图中的light。 
3.   利用服务端Topic路由服务接口，[添加路由关系](https://help.aliyun.com/document_detail/69910.html?spm=a2c4g.11186623.6.665.fyhApw)，如上图中的路由关系`SrcTopic:/pk2/controlApp/update,DstTopics:/pk1/light/get`。 
4.   手机App获取到目标智能灯的deviceName，然后向物联网平台中的Topic发送指令，图中例子`Topic:/pk2/controlApp/update`。 

    其中指令中格式是JSON，并且带上目标智能灯的deviceName，图中例子为：

    ```
    {“TargetDevice”:”light”,”control”:”data”,….}
    ```

5.   在物联网平台里面配置Topic路由服务，将数据从/pk2/controlApp/update转发/pk1/light/get，这样APP发送的指令就可以通过路由服务路由到相应的目标智能灯上。 

    手机App可以调用服务端接口[删除路由关系](https://help.aliyun.com/document_detail/69926.html?spm=a2c4g.11186623.6.666.bOjzgl)，指令不再发送目标智能灯的deviceName。


