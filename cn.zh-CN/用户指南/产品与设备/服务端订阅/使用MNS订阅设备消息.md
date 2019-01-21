# 使用MNS订阅设备消息 {#task_xy5_wk2_vdb .task}

物联网平台基础版产品支持云端应用通过监听MNS队列，获取设备消息。本章讲解使用MNS订阅设备消息的配置方法。

1.  在物联网平台控制台上，为产品配置服务端订阅，实现平台将消息自动转发至MNS。 

    1.  单击**设备管理** \> **产品**，**查看**刚刚创建的产品。 
    2.  单击服务端订阅，在推送MNS区域，单击**设置**，勾选**设备上报消息**或**设备状态变化通知**。 
        -   设备上报消息，指平台将设备上报的数据自动转发至MNS。
        -   设备状态变化通知，指平台将设备上下线的消息自动推送至MNS。
    3.  订阅完成后，MNS将自动新建一个消息队列。消息队列信息显示在控制台上。 
     ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7461/154805836337625_zh-CN.png)

    配置完成大约1分钟后生效。

2.  通过监听MNS队列，接收设备消息。 

    此处使用MNS Java SDK监听消息，涉及以下信息填写。

    具体操作，请参考[MNS文档](https://help.aliyun.com/document_detail/27508.html)。

    -   在pom.xml文件中，添加如下依赖：

        ```
        <dependency>
            <groupId>com.aliyun.mns</groupId>
            <artifactId>aliyun-sdk-mns</artifactId>
            <version>1.1.8</version>
            <classifier>jar-with-dependencies</classifier>
        </dependency>
        ```

    -   接收消息时，需填入以下信息：

        ```
        CloudAccount account = new CloudAccount( $AccessKeyId, $AccessKeySecret, $AccountEndpoint);
        ```

        -   $AccessKeyId和$AccessKeySecret需替换为阿里云账号访问API的基本信息，可以单击阿里云账号头像找到。
        -   $AccountEndpoint需填写实际的Endpoint值，可以从MNS控制台上获取。
    -   填写接收设备消息的逻辑：

        ```
        MNSClient client = account.getMNSClient(); 
        CloudQueue queue = client.getQueueRef("aliyun-iot-a1xxxxxx8o9"); //参数请输入IoT自动创建的队列名称
         
            while (true) { 
            // 获取消息 
            Message popMsg = queue.popMessage(10); //长轮询等待时间为10秒      
            if (popMsg != null) { 
                System.out.println("PopMessage Body: "+ popMsg.getMessageBodyAsRawString()); //获取原始消息 
                queue.deleteMessage(popMsg.getReceiptHandle()); //从队列中删除消息 
            } else { 
                System.out.println("Continuing"); } }
        
        ```

    -   运行程序，完成对MNS队列的监听。
3.  启动设备，上报消息。 

    可参考[SDK文档](https://help.aliyun.com/document_detail/96624.html)，查看设备上报消息的具体内容。

4.  检查云端应用是否监听到设备消息。若成功监听，将获得如下所示消息代码。 

    ```
    {
    "messageid":" ",//消息标识
    "messagetype":"upload",
    "topic":"//信息来源Topic,
    "payload": //Base64编码后的数据
    "timestamp": //时间戳
    }
    ```

    |参数|说明|
    |:-|:-|
    |messageid|物联网平台生成的消息ID，19位大小。|
    |messagetype|消息类型。    -   status：设备状态通知
    -   upload：设备上报消息
|
    |topic|云端监听到的信息来自哪个Topic。    -   当messagetype=status时，为null
    -   当messagetype=upload时，为具体Topic
|
    |payload|Base64编码的数据。    -   当messagetype=status时，数据是平台的通知数据，数据格式为：

        ```
{
    "status":"online|offline",//设备状态
    "productKey":"12345565569",//产品唯一标识，ProductKey
    "deviceName":"deviceName1234",//设备名称
    "time":"2018-08-31 15:32:28.205",//发送通知时间点
    "utcTime":"2018-08-31T07:32:28.205Z",//发送通知UTC时间点 
    "lastTime":"2018-08-31 15:32:28 .195",//状态变更时最后一次通信时间 
    "utcLastTime":"2018-08-31T07:32:28.195Z",//状态变更时最后一次通信UTC时间
    "clientIp":"xxx.xxx.xxx.xxx"//设备端公网出口IP
        ```

**说明：** 为避免消息时序紊乱造成影响，建议您根据lastTime来维护最终设备状态。

    -   当messagetype=upload时，数据是设备发布到Topic中的原始数据
|
    |timestamp|时间戳，以Epoch时间表示。|


