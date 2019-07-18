# Use custom topics for communication {#concept_423001 .concept}

You can define custom topic categories in IoT Platform. Then, a device can send messages to a custom topic, and your server can receive the messages through the HTTP/2 SDK. Your server can also call the API operation [Pub](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md#) to send commands to the device.

## Scenario {#section_ict_9m2_51s .section}

In this example, an electronic thermometer periodically exchanges data with a server. The thermometer sends the current temperature to the server, and the server sends the precision setting command to the thermometer.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/341841/156341764748433_en-US.png)

## Prepare the development environment {#section_1bq_xtz_3q6 .section}

In this example, both the devices and the server use Java SDKs, so you need to prepare the Java development environment first. You can download Java tools at [Java official website](http://developers.sun.com/downloads/) and install the Java development environment.

Add the following Maven dependencies to import the device SDK \(Link Kit Java SDK\) and IoT SDK:

``` {#d7e45}
<dependencies>
 <dependency>
     <groupId>com.aliyun.alink.linksdk</groupId>
     <artifactId>iot-linkkit-java</artifactId>
     <version>1.2.0.1</version>
     <scope>compile</scope>
 </dependency>
 < dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-core</artifactId>
      <version>3.7.1</version>
  </dependency>
  <dependency>
      <groupId>com.aliyun</groupId>
      <artifactId>aliyun-java-sdk-iot</artifactId>
      <version>6.9.0</version>
  </dependency>
  <dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.2</version>
</dependency>
</dependencies>
```

## Create a product and a device {#section_rrw_0cb_w3b .section}

First, you need to create a product, define custom topic categories, define the TSL model, configure service subscription, and create a device in the IoT Platform console.

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).
2.  In the left-side navigation pane, choose **Devices** \> **Product**.
3.  Click **Create Product** to create a thermometer product.

    For more information, see [Create a product](../../../../intl.en-US/User Guide/Create products and devices/Create a product.md#).

4.  After the product is created, find the product and click **View**.
5.  On the Topic Categories tab of the Product Details page, add custom topic categories.

    For more information, see [Create a topic category](../../../../intl.en-US/User Guide/Create products and devices/Topics/Create a topic category.md#).

    In this example, add the following two topic categories:

    -   /$\{productKey\}/$\{deviceName\}/user/devmsg: used by devices to publish messages. Set Device Operation Authorizations to Publish for this topic category.
    -   /$\{productKey\}/$\{deviceName\}/user/cloudmsg: used by devices to receive subscribed messages. Set Device Operation Authorizations to Subscribe for this topic category.
6.  On the Service Subscription tab, set the type of messages to be pushed to the HTTP/2 SDK to **Device Upstream Notification**.

    For more information, see [Development guide for Java HTTP/2 SDK](../../../../intl.en-US/User Guide/Create products and devices/Service Subscription/Development guide for Java HTTP__2 SDK.md#).

    ![](images/50122_en-US.png)

7.  In the left-side navigation pane, choose **Devices** and add a thermometer device under the thermometer product that has been created. For more information, see [Create a device](../../../../intl.en-US/User Guide/Create products and devices/Create devices/Create a device.md#).

## The server receives messages from the device {#section_1oo_pqf_80d .section}

The following figure shows how the device sends a message to the server.

![](images/48448_en-US.png)

-   Configure the HTTP/2 SDK, which will be installed in the server, and set a callback for the specified topic.

    After service subscription is configured, the server listens to all the messages sent by all the devices under the product.

    -   Connect the HTTP/2 SDK to IoT Platform.

        ``` {#codeblock_n3y_vf6_trl}
        // Configure identity verification information.
        String accessKey = "The AccessKey ID of your Alibaba Cloud account";
        String accessSecret = "The AccessKey Secret of your Alibaba Cloud account";
        String uid = "Your Alibaba Cloud account ID";
        String regionId = "The ID of the region where the device is located";
        String productKey = "The key of the product";
        String deviceName = "The name of the device";
        String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";
        // Configure the connection.
        Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);
        
        // Construct a client.
        MessageClient client = MessageClientFactory.messageClient(profile);
        
        // Receive the message.
        client.connect(new MessageCallback() {
            @Override
            public Action consume(MessageToken messageToken) {
                Message m = messageToken.getMessage();
                System.out.println("\ntopic=" + m.getTopic());
                System.out.println("payload=" + new String(m.getPayload()));
                System.out.println("generateTime=" + m.getGenerateTime());
                // CommitSuccess indicates that the current message has been consumed. In this case, IoT Platform will delete the message. If the message is not marked with CommitSuccess, IoT Platform will retain the message until it expires.
                return MessageCallback.Action.CommitSuccess;
             }
        });
        ```

    -   Configure the message receiving interface.

        In this example, the server subscribes to the topic /$ \{productKey\}/$ \{deviceName\}/user/devmsg. Therefore, you need to set a callback for this topic.

        ``` {#codeblock_s48_s7v_pcb}
        // Define the callback.
        MessageCallback messageCallback = new MessageCallback() {
        // Obtain and display the message. You can set the required action for the server here.
                    @Override
                    public Action consume(MessageToken messageToken) {
                        Message m = messageToken.getMessage();
                        log.info("receive : " + new String(messageToken.getMessage().getPayload()));
                        System.out.println(messageToken.getMessage());
                        return MessageCallback.Action.CommitSuccess;
                    }
                };
                // Register the callback.
                client.setMessageListener("/" + productKey + "/" + deviceName + "/user/devmsg", messageCallback);
        ```

    For more information, see [Development guide for Java HTTP/2 SDK](../../../../intl.en-US/User Guide/Create products and devices/Service Subscription/Development guide for Java HTTP__2 SDK.md#).

-   Configure the device SDK to send a message.
    -   Configure device authentication information.

        ``` {#codeblock_8a0_qm6_mq4}
        final String productKey = "XXXXXX";
        final String deviceName = "XXXXXX";
        final String deviceSecret = "XXXXXXXXX";
        final String region = "XXXXXX";
        ```

    -   Set connection initialization parameters, including MQTT connection information, device information, and initial device status.

        ``` {#codeblock_brg_alz_ra5}
        LinkKitInitParams params = new LinkKitInitParams();
        // Configure MQTT connection information. Link Kit uses MQTT as the underlying protocol. 
        IoTMqttClientConfig config = new IoTMqttClientConfig();
        config.productKey = productKey;
        config.deviceName = deviceName;
        config.deviceSecret = deviceSecret;
        config.channelHost = productKey + ".iot-as-mqtt." + region + ".aliyuncs.com:1883";
        // Configure device information.
        DeviceInfo deviceInfo = new DeviceInfo();
        deviceInfo.productKey = productKey;
        deviceInfo.deviceName = deviceName;
        deviceInfo.deviceSecret = deviceSecret;
        // Register the initial device status.
        Map<String, ValueWrapper> propertyValues = new HashMap<String, ValueWrapper>();
        
        params.mqttClientConfig = config;
        params.deviceInfo = deviceInfo;
        params.propertyValues = propertyValues;
        ```

    -   Initialize the connection.

        ``` {#codeblock_lrf_1dy_vcg}
        // Initialize the connection and set the callback that is called when the initialization is successful.
        LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
             @Override
             public void onError(AError aError) {
                 System.out.println("Init error:" + aError);
             }
        
             // Set the callback that is called when the initialization is successful.
             @Override
             public void onInitDone(InitResult initResult) {
                 System.out.println("Init done:" + initResult);
             }
         });
        ```

    -   Send a message from the device.

        After connecting to IoT Platform, the device sends a message to the specified topic. Replace the content of the onInitDone callback like the following example:

        ``` {#codeblock_itg_4gg_t1v}
        @Override
         public void onInitDone(InitResult initResult) {
             // Set the topic to which the message is published and the message content.
             MqttPublishRequest request = new MqttPublishRequest();
             request.topic = "/" + productKey + "/" + deviceName + "/user/devmsg";
             request.qos = 0;
             request.payloadObj = "{\"temperature\":35.0, \"time\":\"sometime\"}";
             // Publish the message and set the callback that is called when the message is published.
             LinkKit.getInstance().publish(request, new IConnectSendListener() {
                 @Override
                 public void onResponse(ARequest aRequest, AResponse aResponse) {
                     System.out.println("onResponse:" + aResponse.getData());
                 }
        
                 @Override
                 public void onFailure(ARequest aRequest, AError aError) {
                     System.out.println("onFailure:" + aError.getCode() + aError.getMsg());
                 }
             });
         }
        ```

        The server receives the following message:

        ``` {#codeblock_7m2_jrb_8w0}
        Message
        {payload={"temperature":35.0, "time":"sometime"},
        topic='/a1uzcH0****/device1/user/devmsg',
        messageId='1131755639450642944',
        qos=0,
        generateTime=1558666546105}
        ```


## The server sends messages to the device {#section_bhe_qsr_tpd .section}

The following figure shows how the server sends a message to the device.

![](images/48453_en-US.png)

-   Configure the device SDK to subscribe to a topic.

    For more information about how to configure device authentication information, set connection initialization parameters, and initialize the connection, see the device SDK configuration in the previous section.

    The device needs to subscribe to a specific topic to receive messages sent by the server.

    The following example demonstrates how to configure the device SDK to subscribe to a topic:

    ``` {#codeblock_s17_y9x_qty}
    // Set the callback that is called when the initialization is successful.
    @Override
    public void onInitDone(InitResult initResult) {
        // Set the topic to which the device subscribes.
        MqttSubscribeRequest request = new MqttSubscribeRequest();
        request.topic = "/" + productKey + "/" + deviceName + "/user/cloudmsg";
        request.isSubscribe = true;
        // Send a subscription request and set the callbacks that are called when the subscription succeeds and fails, respectively.
        LinkKit.getInstance().subscribe(request, new IConnectSubscribeListener() {
            @Override
            public void onSuccess() {
                System.out.println("");
            }
    
            @Override
            public void onFailure(AError aError) {
    
            }
        });
    
        // Set the listener for listening to subscribed messages.
        IConnectNotifyListener notifyListener = new IConnectNotifyListener() {
            // Define the callback that is called when a subscribed message is received.
            @Override
            public void onNotify(String connectId, String topic, AMessage aMessage) {
                System.out.println(
                    "received message from " + topic + ":" + new String((byte[])aMessage.getData()));
            }
    
            @Override
            public boolean shouldHandle(String s, String s1) {
                return false;
            }
    
            @Override
            public void onConnectStateChange(String s, ConnectState connectState) {
    
            }
        };
        LinkKit.getInstance().registerOnNotifyListener(notifyListener);
    }
    ```

-   Configure the IoT SDK to call the [Pub](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Communications/Pub.md#) operation to publish a message.
    -   Configure identity verification information.

        ``` {#codeblock_wam_f4n_4sh}
         String regionId = "The ID of the region where the device is located";
         String accessKey = "The AccessKey ID of your Alibaba Cloud account";
         String accessSecret = "The AccessKey Secret of your Alibaba Cloud account";
         final String productKey = "The key of the product";
        ```

    -   Set connection parameters.

        ``` {#codeblock_1o2_ikh_7gy}
        // Construct a client.
        DefaultProfile profile = DefaultProfile.getProfile(regionId, accessKey, accessSecret);
        IAcsClient client = new DefaultAcsClient(profile);
        ```

    -   Set the parameters for publishing a message.

        ``` {#codeblock_c93_fm9_b28}
        PubRequest request = new PubRequest();
        request.setQos(0);
        // Set the topic to which the message is published.
        request.setTopicFullName("/" + productKey + "/" + deviceName + "/user/cloudmsg");
        request.setProductKey(productKey);
        // Set the message content. The message content must be encoded in Base64. Otherwise, the message content will be garbled characters.
        request.setMessageContent(Base64.encode("{\"accuracy\":0.001,\"time\":now}"));
        ```

    -   Publish the message.

        ``` {#codeblock_obb_hue_hkl}
        try {
             PubResponse response = client.getAcsResponse(request);
             System.out.println("pub success?:" + response.getSuccess());
         } catch (Exception e) {
             System.out.println(e);
         }
        ```

        The device receives the following message:

        ``` {#codeblock_nj8_u17_4rj}
        msg = [{"accuracy":0.001,"time":now}]
        ```


## Appendix: demo {#section_y5z_eyd_h0d .section}

Click [here](https://iotx-demo.oss-cn-hangzhou.aliyuncs.com/PubSubDemo.zip) to download and view the complete demo for this example.

