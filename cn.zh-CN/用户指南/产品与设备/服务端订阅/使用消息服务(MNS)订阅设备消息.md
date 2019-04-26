# 使用消息服务\(MNS\)订阅设备消息 {#task_xy5_wk2_vdb .task}

物联网平台服务端订阅支持将设备消息发送至消息服务\(MNS\)，云端应用通过监听MNS队列，获取设备消息。本章讲解使用MNS订阅设备消息的配置方法。

1.  在物联网平台控制台上，为产品配置服务端订阅，实现物联网平台将消息自动转发至MNS。 

    1.  在产品的产品详情页，选择**服务端订阅**页签。 
    2.  单击服务端订阅（推送MNS）对应的**设置**按钮。 
    3.  在服务端订阅对话框中，选择要推送的消息类型后，单击**保存**。 
    订阅完成后，物联网平台会自动创建MNS消息队列，名称格式为`aliyun-iot-${yourProductKey}`。如果已存在相关消息队列，则不再创建。该消息队列信息将显示在服务端订阅页。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7461/155627064537625_zh-CN.png)

2.  配置监听MNS队列，以接收设备消息。 

    具体操作，请参考[MNS文档](https://help.aliyun.com/document_detail/27508.html)。

    以下示例中，使用MNS Java SDK监听消息。涉及以下配置：

    -   在pom.xml文件中，添加如下依赖：

        ```
        <dependency>
            <groupId>com.aliyun.mns</groupId>
            <artifactId>aliyun-sdk-mns</artifactId>
            <version>1.1.8</version>
            <classifier>jar-with-dependencies</classifier>
        </dependency>
        ```

    -   配置接收消息时，需填入以下信息：

        ```
        CloudAccount account = new CloudAccount( $AccessKeyId, $AccessKeySecret, $AccountEndpoint);
        ```

        -   $AccessKeyId和$AccessKeySecret需替换为您的阿里云账号访问API的基本信息。登录阿里云控制台，光标移至阿里云账号头像上，然后单击**AccessKey管理**，创建或查看AccessKey信息。
        -   $AccountEndpoint需填写实际的Endpoint值。在MNS控制台，单击**获取Endpoint**获取。
    -   填写接收设备消息的逻辑：

        ```
        MNSClient client = account.getMNSClient(); 
        CloudQueue queue = client.getQueueRef("aliyun-iot-a1xxxxxx8o9"); //请输入IoT自动创建的队列名称
         
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

    设备端SDK开发，请参考[Link Kit SDK文档](https://help.aliyun.com/document_detail/96624.html)。

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
    |messageid|物联网平台生成的消息ID。|
    |messagetype|消息类型。    -   status：设备状态通知
    -   upload：设备上报消息
    -   device\_lifecycle：设备生命周期变更通知
    -   topo\_lifecycle：设备拓扑关系变更
    -   topo\_listfound：网关发现子设备上报
|
    |topic|服务端监听到的信息来源的物联网平台Topic。|
    |payload|Base64编码的消息数据。payload数据格式，请参见[数据格式](cn.zh-CN/用户指南/规则引擎/数据流转/数据格式.md#)。

|
    |timestamp|时间戳，以Epoch时间表示。|


