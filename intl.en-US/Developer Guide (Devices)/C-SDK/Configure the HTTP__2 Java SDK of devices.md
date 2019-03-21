# Configure the HTTP/2 Java SDK of devices {#concept_hy2_5zr_s2b .concept}

You can use the HTTP/2 protocol to establish connections between your devices and IoT Platform. This topic describes an HTTP/2 Java SDK development demo that you can use as reference to help you develop your own HTTP/2 Java SDK of devices.

## Prerequisites {#section_eln_b1s_s2b .section}

In this Java demo, a Maven project is used. Before you begin, make sure that you have installed Maven.

## Procedure {#section_myy_xcs_s2b .section}

1.  Download the Java service subscription SDK at the following link: [iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip).
2.  Use IntelliJ IDEA or Eclipse to import the demo into a Maven project.
3.  Obtain the ProductKey, DeviceName, and DeviceSecret of a device from the IoT Platform console. For more information, see **User Guide** \> **Create products and devices**.
4.  Modify the H2Client.java configuration file.
    1.  Configure the parameters.

        ```
        //Obtain the ProductKey, DeviceName, and DeviceSecret of the device from the IoT Platform console.
        String productKey = "";
        String deviceName = "";
        String deviceSecret = "";
        
        //The topics that are used to receive and send messages.
        String subTopic = "/" + productKey + "/" + deviceName + "/get";
        String pubTopic = "/" + productKey + "/" + deviceName + "/update";
        ```

    2.  Connect the HTTP/2 SDK to the HTTP/2 server and configure data receiving.

        ```
        //endPoint: https://${YourProductKey}.iot-as-http2.${region}.aliyuncs.com
        String endPoint = "https://" + productKey + ".iot-as-http2.cn-shanghai.aliyuncs.com";
        
        //The unique identifier of the client device
        String clientId = InetAddress.getLocalHost().getHostAddress();
        
        //Configure connection parameters
        Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
        
        //A value of true indicates that all messages that are sent when the device is offline will be cleared. Offline messages refer to all QoS 1 or QoS 2 messages that are not received.
        profile.setCleanSession(false);
        
        //Construct the client
        MessageClient client = MessageClientFactory.messageClient(profile);
        
        //Receive data
        client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
        });
        ```

    3.  Subscribe to topics.

        ```
        //Subscribe to a topic After the HTTP/2 SDK is connected to IoT Platform, the HTTP/2 SDK can receive messages from the specified topic in a callback.
        CompletableFuture subFuture = client.subscribe(subTopic);
        System.out.println("sub result : " + subFuture.get());
        ```

    4.  Send data.

        ```
        //Publish a message
        MessageToken messageToken = client.publish(pubTopic, new Message("hello iot".getBytes(), 0));
        System.out.println("publish success, messageId: " + messageToken.getPublishFuture().get().getMessageId());
        ```

5.  Run the code after you have modified the configuration file.

## API descriptions {#section_onn_j2s_s2b .section}

-   Identity authentication

    To connect a device to IoT Platform, you can use Profile to configure identity information of the device. Configure the API parameters as follows:

    ```
    Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
    MessageClient client = MessageClientFactory.messageClient(profile);
    client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
    });
    ```

    Parameters in Profile

    |Parameter|Data type|Required?|Description|
    |:--------|:--------|:--------|:----------|
    |endPoint|String|Yes|The endpoint, in the format of `https://$\{YourProductKey\}.iot-as-http2.$\{regionId\}.aliyuncs.com`.The parameters are described as follows:

    -   Replace $\{YourProductKey\} with the ProductKey value of the device.
    -   Replace $\{regionId\} with the ID of your service region. For region IDs, see [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm).
|
    |productKey|String|Yes|The unique identifier of the product to which the device belongs. You can obtain this information from the IoT Platform console.|
    |deviceName|String|Yes|The name of the device. You can obtain this information from the IoT Platform console.|
    |deviceSecret|String|Yes|The key of the device. You can obtain this information from the IoT Platform console.|
    |clientId|String|Yes|The unique identifier of the client device.|
    |cleanSession|Boolean|No|Indicates whether to clear cached messages that are sent when the device is offline.|
    |heartBeatInterval|Long|No|The heartbeat interval, in milliseconds.|
    |heartBeatTimeOut|Long|No|The heartbeat timeout time, in milliseconds.|
    |multiConnection|Boolean|No|Indicates whether to enable multi-connections. If the ProductKey and DeviceName of the device are used to connect to IoT Platform, set the value of this parameter to false.|
    |callbackThreadCorePoolSize|Integer|No|The size of the core pool for the callback thread.|
    |callbackThreadMaximumPoolSize|Integer|No|The maximum number of threads for the callback thread pool.|
    |callbackThreadBlockingQueueSize|Integer|No|The size of the blocking queue for the callback thread pool.|
    |authParams|Map|No|The custom authentication parameters.|

-   Connection establishment

    After MessageClient is generated, call the connection establishment API to connect the HTTP/2 SDK to IoT Platform. After the device is connected to IoT Platform, it can send messages to IoT Platform. IoT Platform will immediately push the subscribed messages to the corresponding HTTP/2 SDK on your server. The connection establishment API is as follows:

    ```
    void connect(MessageCallback messageCallback);
    ```

-   Message subscription

    ```
    /**
    * Subscribe to a topic
    * @param  topic              topic
    * @return completableFuture for subscribe result
    */
    
    CompletableFuture subscribe(String topic);
    
    /**
    * Subscribe to a topic and specify the callback for the topic 
    * @param  topic              topic
    * @param messageCallback callback when message received on this topic
    * @return completableFuture for subscribe result
    */
    CompletableFuture subscribe(String topic, MessageCallback messageCallback);
    
    /**
    * Unsubscribe from the topic
    *
    * @param  topic               topic
    * @return completableFuture for unsubscribe result
    */
    CompletableFuture unsubscribe(String topic);
    ```

-   Message receiving

    Before you configure message receiving, you must configure the MessageCallback method. Additionally, you must pass in MessageClient when configuring connection parameters or subscribing to a topic. The message receiving API is as follows:

    ```
    /**
    * 
    * @param messageToken  message token
    * @return Action        action after consuming
    */
    Action consume(final MessageToken messageToken);
    ```

    This API will be called after the HTTP/2 SDK receives messages by calling MessageToken.getMessage. APIs are called in the thread pool. Pay attention to security issues of the threads. The return values determine whether to return ACKs for messages sent with QoS 1 and QoS2. The return values are described as follows:

    |Return value|Description|
    |------------|-----------|
    |Action.CommitSuccess|An ACK will be returned.|
    |Action.CommitFailure|No ACK will be returned, and the message will be received again later.|
    |Action.CommitAckManually|An ACK will not be returned automatically. You must call MessageClient.ack\(\) to return an ACK manually.|

-   Message sending

    ```
    /**
    * Publish a message to the specified topic
    *
    * @param  topic              message topic
    * @param  message            message entity
    * @return completableFuture for publish result
    */
    MessageToken publish(String topic, Message message);
    ```


