# 使用HTTP/2 SDK（Java）建连 {#concept_hy2_5zr_s2b .concept}

物联网平台提供HTTP/2 SDK（Java） ，用于建立设备端与物联网平台的通信。本文提供SDK Demo。您可以参考此Demo，开发SDK，设备端消息上传至物联网平台。

## 前提条件 {#section_eln_b1s_s2b .section}

此Java Demo为Maven工程，请先安装Maven。

## 操作步骤 {#section_myy_xcs_s2b .section}

1.  下载HTTP/2 SDK（Java） Demo。下载地址为：[iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip)。
2.  使用IDEA或者Eclipse，将该Demo导入到工程里面。
3.  从控制台获取设备证书信息。具体请参考**用户指南**\>**创建产品与设备**相关文档。
4.  修改H2Client.java配置文件。
    1.  配置参数。

        ``` {#codeblock_uy5_v5n_juw}
        // TODO 从控制台获取productKey、deviceName、deviceSecret信息
        String productKey = "";
        String deviceName = "";
        String deviceSecret = "";
        
        // 用于接收消息的topic
        String subTopic = "/" + productKey + "/" + deviceName + "/get";
        String pubTopic = "/" + productKey + "/" + deviceName + "/update";
        ```

    2.  连接HTTP/2服务器，并接收数据。

        ``` {#codeblock_yys_oyg_8sc}
        // endPoint: https://${YourProductKey}.iot-as-http2.${region}.aliyuncs.com
        String endPoint = "https://" + productKey + ".iot-as-http2.cn-shanghai.aliyuncs.com";
        
        // 客户端设备唯一标记
        String clientId = InetAddress.getLocalHost().getHostAddress();
        
        // 连接配置
        Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
        
        // 如果是true那么清理所有离线消息，即qos0或者1的所有未接收内容
        profile.setCleanSession(false);
        
        // 构造客户端
        MessageClient client = MessageClientFactory.messageClient(profile);
        
        // 数据接收
        client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
        });
        ```

    3.  订阅Topic。

        ``` {#codeblock_rcr_mpm_3ta}
        // topic订阅。订阅成功后，即可在建连时的回调接口中收到消息
        CompletableFuture subFuture = client.subscribe(subTopic);
        System.out.println("sub result : " + subFuture.get());
        ```

    4.  发送数据。

        ``` {#codeblock_duv_8hu_8sn}
        // 发布消息
        MessageToken messageToken = client.publish(pubTopic, new Message("hello iot".getBytes(), 0));
        System.out.println("publish success, messageId: " + messageToken.getPublishFuture().get().getMessageId());
        ```

5.  配置修改完成后，直接运行。

## 接口说明 {#section_onn_j2s_s2b .section}

-   身份认证

    设备连接物联网平台时，需要使用Profile配置设备身份及相关参数。具体接口参数如下：

    ``` {#codeblock_tj0_7z3_cr2}
    Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
    MessageClient client = MessageClientFactory.messageClient(profile);
    client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
    });
    ```

    Profile参数说明

    |名称|类型|是否必须|描述|
    |:-|:-|:---|:-|
    |endPoint|String|是|接入点地址，格式为`https://$\{YourProductKey\}.iot-as-http2.$\{regionId\}.aliyuncs.com`。     -   变量$\{YourProductKey\}替换为您的产品Key；
    -   变量$\{regionId\}替换为您的服务地域代码。地域代码表达方法，请参见[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。
 |
    |productKey|String|是|设备所隶属产品的Key，可从控制台获取。|
    |deviceName|String|是|设备的名称，可从控制台获取。|
    |deviceSecret|String|是|设备的密钥，可从控制台获取。|
    |clientId|String|是|客户端设备的唯一标识。|
    |cleanSession|Boolean|否|是否清除缓存的离线消息。|
    |heartBeatInterval|Long|否|心跳间隔，单位为毫秒。|
    |heartBeatTimeOut|Long|否|心跳超时时间，单位为毫秒。|
    |multiConnection|Boolean|否|是否使用多连接。若使用设备的productKey和deivceName接入时，请将此参数值设置为false。|
    |callbackThreadCorePoolSize|Integer|否|回调线程的corePoolSize（核心池的大小）。|
    |callbackThreadMaximumPoolSize|Integer|否|回调线程池的maximumPoolSize（最大线程数）。|
    |callbackThreadBlockingQueueSize|Integer|否|回调线程池的BlockingQueueSize（阻塞队列大小）。|
    |authParams|Map|否|自定义认证参数。|

-   建立连接

    获取MessageClient之后需要与物联网平台建立连接。身份认证成功后才可收发消息。连接建立成功，服务端会立即向SDK推送已订阅的消息，因此建连时需要提供默认消息接收接口，用于处理未设置回调的消息。建连接口如下：

    ``` {#codeblock_qvq_urb_gnx}
    void connect(MessageCallback messageCallback);
    ```

-   消息订阅

    ``` {#codeblock_bos_8lq_89m}
    /**
    * 订阅topic
    * @param  topic              topic
    * @return completableFuture  for subscribe result
    */
    
    CompletableFuture subscribe(String topic);
    
    /**
    * 订阅topic,并制定该topic回调 
    * @param  topic              topic
    * @param  messageCallback    callback when message is received on this topic
    * @return completableFuture  for subscribe result
    */
    CompletableFuture subscribe(String topic, MessageCallback messageCallback);
    
    /**
    * 取消订阅
    *
    * @param  topic               topic
    * @return completableFuture   for unsubscribe result
    */
    CompletableFuture unsubscribe(String topic);
    ```

-   消息接收

    消息接收需要实现MessageCallback接口，并在连接或订阅Topic时传入MessageClient，具体接口如下：

    ``` {#codeblock_02d_h8d_apg}
    /**
    * 
    * @param  messageToken  message token
    * @return Action        action after consuming
    */
    Action consume(final MessageToken messageToken);
    ```

    通过MessageToken.getMessage获取消息后，该方法会被调用。因接口在线程池中调用，所以请注意线程安全问题。该方法返回值决定了QoS0及QoS1消息是否回复的ACK。返回值说明如下：

    |返回值|描述|
    |---|--|
    |Action.CommitSuccess|回执ACK。|
    |Action.CommitFailure|不回执ACK，稍后会重新收到消息。|
    |Action.CommitAckManually|不回执ACK，需要手动调用MessageClient.ack\(\) 方法回复。|

-   消息发送

    ``` {#codeblock_g1o_fei_cf4}
    /**
    * 发送消息到指定topic
    *
    * @param  topic              message topic
    * @param  message            message entity
    * @return completableFuture  for publish result
    */
    MessageToken publish(String topic, Message message);
    ```


