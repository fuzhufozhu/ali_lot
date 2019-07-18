# Broadcast messages {#concept_388674 .concept}

IoT Platform supports broadcast communication. That is, IoT Platform allows a server to broadcast a message to multiple devices under a product. The devices only need to subscribe the broadcast topic to receive the broadcast message sent by the server. This topic describes how to configure broadcast communication.

## Scenario {#section_ct2_ml9_h5f .section}

You have multiple thermometers connected to IoT Platform, and need to send the same precision setting command to all these thermometers from your server.

All the devices subscribe to the same broadcast topic, and the server calls the PubBroadcast operation to publish the message to this topic.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/315186/156341768948163_en-US.png)

## Prepare the development environment {#section_bgi_2e4_kzd .section}

In this example, both the devices and the server use Java SDKs, so you need to prepare the Java development environment first. You can download Java tools at [Java official website](http://developers.sun.com/downloads/) and install the Java development environment.

Add the following Maven dependencies to import the device SDK \(Link Kit Java SDK\) and IoT SDK:

``` {#codeblock_55a_53l_h2q}
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

## Create a product and devices {#section_bo8_lx0_zs5 .section}

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/).
2.  In the left-side navigation pane, choose **Devices** \> **Product**.
3.  Click **Create Product** to create a thermometer product.

    For more information, see [Create a product](../../../../intl.en-US/User Guide/Create products and devices/Create a product.md#).

4.  In the left-side navigation pane, choose **Devices** and add two thermometer devices under the thermometer product that has been created.

    For more information, see [Create multiple devices at a time](../../../../intl.en-US/User Guide/Create products and devices/Create devices/Create multiple devices at a time.md#).


## Configure the device SDK {#section_oq7_zpn_z36 .section}

Connect the device SDK to IoT Platform and subscribe to the broadcast topic.

-   Connect the device SDK to IoT Platform.
    -   Configure device authentication information.

        ``` {#codeblock_kjq_rlt_juu}
        final String productKey = "XXXXXX";
        final String deviceName = "XXXXXX";
        final String deviceSecret = "XXXXXXXXX";
        final String region = "XXXXXX";
        ```

        productKey, deviceName, and deviceSecret: device authentication information. You can obtain the values of these parameters on the device details page in the IoT Platform console.

        region: the ID of the region where the device is located. For more information about region IDs, see [Regions and zones](../../../../intl.en-US/General Reference/Regions and zones.md#).

    -   Set connection initialization parameters, including MQTT connection information, device information, and initial device status.

        ``` {#codeblock_u11_2ux_7vh}
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

        ``` {#codeblock_dxl_jvf_0rj}
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

-   Subscribe to the broadcast topic. Set the onInitDone callback to send a subscription request.

    You do not need to create the broadcast topic in advance. Instead, directly enter the broadcast topic in the format of /broadcast/$\{productKey\}/Custom field.

    In this example, the broadcast topic is `"/broadcast/" + productKey + "/userDefine"`.

    **Note:** The same broadcast topic must be configured in the device SDK and IoT SDK.

    ``` {#codeblock_9k3_mx8_mqv}
    public void onInitDone(InitResult initResult) {
          // Set the broadcast topic to which the device subscribes. You do not need to create this topic in the IoT Platform console.
         MqttSubscribeRequest request = new MqttSubscribeRequest();
         request.topic = "/broadcast/" + productKey + "/userDefine";
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
         // Register the listener for listening to subscribed messages.
         LinkKit.getInstance().registerOnNotifyListener(notifyListener);
     }
    ```


## Configure the IoT SDK {#section_di3_xra_ka8 .section}

Configure the IoT SDK to send a broadcast message.

-   Configure identity verification information.

    ``` {#codeblock_437_lud_vt3}
     String regionId = "The ID of the region where the device is located";
     String accessKey = "The AccessKey ID of your Alibaba Cloud account";
     String accessSecret = "The AccessKey Secret of your Alibaba Cloud account";
     final String productKey = "The key of the product";
    ```

-   Configure the IoT SDK to call the [PubBroadcast](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Communications/PubBroadcast.md#) operation to publish a broadcast message.

    ``` {#codeblock_wqq_b5e_hft}
    // Construct a client.
     DefaultProfile profile = DefaultProfile.getProfile(regionId, accessKey, accessSecret);
     IAcsClient client = new DefaultAcsClient(profile);
    
     PubBroadcastRequest request = new PubBroadcastRequest();
     // Set the topic to which the broadcast message is published. This topic must be same as the broadcast topic to which the device subscribes.
     request.setTopicFullName("/broadcast/" + productKey + "/userDefine");
     request.setProductKey(productKey);
     // Set the message content. The message content must be encoded in Base64. Otherwise, the message content will be garbled characters.
     request.setMessageContent(Base64.encode("{\"accuracy\":0.001,\"time\":now}"));
    ```

-   Publish the broadcast message.

    ``` {#codeblock_0cq_acd_93j}
    try {
        PubBroadcastResponse response = client.getAcsResponse(request);
        System.out.println("broadcast pub success?:" + response.getSuccess());
    } catch (Exception e) {
        System.out.println(e);
    }
    ```


## Verification {#section_ski_b0w_v7r .section}

Verify that the message broadcast by the IoT SDK to devices is `"{\"accuracy\":0.001,\"time\":now}"`.

In logs of the two thermometers, verify that the message `{"accuracy":0.001,"time":now}` is received.

## Appendix: demo {#section_r67_iwn_6jd .section}

Click [here](https://iotx-demo.oss-cn-hangzhou.aliyuncs.com/BroadcastDemo.zip) to download and view the complete demo for this example.

