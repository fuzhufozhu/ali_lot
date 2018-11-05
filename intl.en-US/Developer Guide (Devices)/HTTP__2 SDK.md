# HTTP/2 SDK {#concept_hy2_5zr_s2b .concept}

You can use the HTTP/2 protocol to establish communication between your devices and IoT Platform. The following is an HTTP/2 SDK development demo that you can use as reference to help you develop your own HTTP/2 SDK.

## Prerequisites {#section_eln_b1s_s2b .section}

In this demo, a Maven project is used. You must have installed Maven before you begin.

## Procedure {#section_myy_xcs_s2b .section}

1.  Download the HTTP/2 SDK at the following link: [iot-http2-sdk-demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/iot-http2-sdk-demos.zip).
2.  Use IntelliJ IDEA or Eclipse to import the demo into a Maven project.
3.  Obtain the device information \(ProductKey, DeviceName, and DeviceSecret\) on the IoT Platform console. For more information, see **User Guide**\>**Create products and devices**.
4.  Modify the H2Client.java configuration file.
    1.  Configure the parameters.

        ```
        //Obtain the ProductKey, DeviceName, and DeviceSecret of the device from the IoT Platform console.
        String productKey = "";
        String deviceName = "";
        String deviceSecret = "";
        
        // The topics that are used to receive messages.
        String subTopic = "/" + productKey + "/" + deviceName + "/get";
        String pubTopic = "/" + productKey + "/" + deviceName + "/update";
        ```

    2.  Connect to the HTTP/2 server to receive data.

        ```
        // endPoint: https://${uid}.iot-as-http2.${region}.aliyuncs.com
        String endPoint = "https://" + productKey + ".iot-as-http2.cn-shanghai.aliyuncs.com";
        
        // The unique identifier of the client device.
        String clientId = InetAddress.getLocalHost().getHostAddress();
        
        // Configure for device connection
        Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
        
        //A value of true indicates that all offline messages will be cleared. Offline messages refer to all messages that were not received when messages were sent with QoS 1 or QoS 2.
        profile.setCleanSession(false);
        
        //Construct the client
        MessageClient client = MessageClientFactory.messageClient(profile);
        
        // Receive data
        client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
        });
        ```

    3.  Subscribe to topics.

        ```
        // Subscribe to topics. After the topic subscription is successful, you can receive messages by using the callback interface when the device is connected.
        CompletableFuture subFuture = client.subscribe(subTopic);
        System.out.println("sub result : " + subFuture.get());
        ```

    4.  Send data.

        ```
        //Send messages.
        MessageToken messageToken = client.publish(pubTopic, new Message("hello iot".getBytes(), 0));
        System.out.println("publish success, messageId: " + messageToken.getPublishFuture().get().getMessageId());
        ```

5.  Run the code after you have modified the configuration file.

## Interface description {#section_onn_j2s_s2b .section}

-   Identity authentication interface

    When you connect devices to IoT Platform, you can use Profile to configure identity information of the devices. The interface parameters are as follows:

    ```
    Profile profile = Profile.getDeviceProfile(endPoint, productKey, deviceName, deviceSecret, clientId);
    MessageClient client = MessageClientFactory.messageClient(profile);
    client.connect(messageToken -> {
        Message m = messageToken.getMessage();
        System.out.println("receive message from " + m);
        return MessageCallback.Action.CommitSuccess;
    });
    ```

    Description of parameters in Profile.

    |Name|Type|Required|Description|
    |----|----|--------|-----------|
    |endPoint|String|Yes|The endpoint address. The endpoint format is: https://$\{productKey\}.iot-as-http2.$\{regionId\}.aliyuncs.com. For example, https://al2sdABC1234.iot-as-http2.cn-shanghai.aliyuncs.com|
    |productKey|String|Yes|The ID of the product to which the device belongs. You can get this information on the IoT Platform console.|
    |deviceName|String|Yes|The name of the device. You can get this information on the IoT Platform console.|
    |deviceSecret|String|Yes|The secret of the device. You can get this information on the IoT Platform console.|
    |clientId|String|Yes|The unique identifier of the client device.|
    |cleanSession|Boolean|No|Determines whether or not to clear the messages that were not received when they were sent with QoS 1 or QoS 2.|
    |heartBeatInterval|Long|No|The interval of heartbeat, in milliseconds.|
    |heartBeatTimeOut|Long|No|The timeout time of a heartbeat, in milliseconds.|
    |multiConnection|Boolean|No|Determines whether or not to use multiple connections. If the device productKey and deviceName are used for device connection, set this parameter value as false.|
    |callbackThreadCorePoolSize|Integer|No|The size of the core callback thread pool.|
    |callbackThreadMaximumPoolSize|Integer|No|The maximum size of the callback thread pool.|
    |callbackThreadBlockingQueueSize|Integer|No|The blocking queue of the callback thread.|
    |authParams|Map|No|Custom authentication parameters.|

-   Connect to IoT Platform

    After the MessageClient value is received, you can connect a device to IoT Platform. IoT Platform will then authenticate the device identity and, if authentication is successful, the messages will be sent and received. When you are configuring the connection, you need to configure a message receiving interface that is able to handle messages without setting a callback interface. Then, the server can push messages that have been subscribed to the SDK. The configuration for the connection is as follows:

    ```
    void connect(MessageCallback messageCallback);
    ```

-   Message subscription

    ```
    /**
    * Subscribe to a topic
    * @param topic                        topic
    * @return completableFuture for subscribe result
    */
    
    CompletableFuture subscribe(String topic);
    
    /**
    * Subscribe to a topic and define the callback topic 
    * @param topic                       topic
    * @param messageCallback callback when message received on this topic
    * @return completableFuture for subscribe result
    */
    CompletableFuture subscribe(String topic, MessageCallback messageCallback);
    
    /**
    * unsubscribe a topic
    *
    * @param topic                        topic
    * @return completableFuture for unsubscribe result
    */
    CompletableFuture unsubscribe(String topic);
    ```

-   Receive messages

    When you set configurations for receiving messages, you must configure the callback interface MessageCallback. Additionally, you must set configurations for the connection and for topic subscription by including the interface MessageClient. The configuration for the message receiving interface is as follows:

    ```
    /**
    * 
    * @param messageToken  message token
    * @return Action                   action after consuming
    */
    Action consume(final MessageToken messageToken);
    ```

    When messages are received by using MessageToken.getMessage, this method will be called. Because the interfaces are called in the thread pool, note the following security issues. The returned values of this method will decide whether or not to return ACKs for messages sent with QoS 1 and QoS2.

    |Return value|Description|
    |------------|-----------|
    |Action.CommitSuccess|ACK will be returned.|
    |Action.CommitFailure|ACK will not be returned, and the message will be received later.|
    |Action.CommitAckManually|ACK will not be returned automatically. Call MessageClient.ack\(\) to return ACK manually.|

-   Publish messages

    ```
    /**
    * Publish a message to a specified topic
    *
    * @param topic                      topic
    * @param message                 message entity
    * @return completableFuture for publish result
    */
    MessageToken publish(String topic, Message message);
    ```


