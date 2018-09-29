# Development guide {#concept_cgg_k2v_y2b .concept}

This article introduces how to configure service subscription, connect to HTTP/2 SDK, authenticate identity, and configure the message-receiving interface.

## Configure service subscription {#section_tbd_2s5_42b .section}

1.  Log on to the [IoT Platform console](https://iot.console.aliyun.com/product/region/cn-shanghai).
2.  In the left-side navigation pane, click **Products**.
3.  In the product list, find the product for which you want to configure service subscription and click **View**. You are directed to the Product Details page.
4.  Click **Service Subscription** \> **Set Now**.
5.  Select the types of notifications that you want to push to the SDK. There are two types: **Device Upstream Notification** and **Device Status Change Notification**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18850/153820507612666_en-US.png)

    -   Device Upstream Notification: Indicates the messages of topics to which devices are allowed to publish messages. If it is selected, the HTTP/2 SDK can receive the messages reported by devices.

        For example, a Pro Edition product has three topic categories:

        -   `/${YourProductKey}/${YourDeviceName}/user/get`, devices can subscribe to messages.
        -   `/${YourProductKey}/${YourDeviceName}/user/update`, devices can publish messages.
        -   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, devices can publish messages.
        Service Subscription can push messages of the topics `/${YourProductKey}/${YourDeviceName}/user/update` and `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, to which devices can publish messages. In addition, the messages of `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post` are processed by the system before being pushed.

    -   Device Status Change Notification: Indicates the notifications that are sent when the statuses of devices change. For example, the notifications on devices going online or going offline. The topic `/as/mqtt/status/${YourProductKey}/${YourDeviceName}` has device status change messages. After this notification type is selected, the HTTP/2 SDK can receive the device status change notifications.

## Connect to the SDK {#section_v3d_gj5_42b .section}

Add maven dependency to the project to connect to the SDK.

```
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.2</version>
</dependency>

<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>3.7.1</version>
</dependency>
```

## Identity authentication {#section_atv_yl5_42b .section}

Use the AccessKey information of your Alibaba Cloud account for identity authentication and to build the connection between the SDK and IoT Platform.

See the following example:

```
String accessKey = "xxxxxxxxxxxxxxxxx";
String accessSecret = "xxxxxxxxxxxxxxx";
String uid = "xxxxxxxxxxxxxxxxx";
String region = "cn-shanghai";
String endPoint = "https://${uid}.iot-as-http2.${region}.aliyuncs.com:443"
Profile profile = new Profile(endPoint, region, accessKey, accessSecret);
MessageClient client = MessageClientFactory.messageClient(profile);
client.connect(messageToken -> {
    Message m = messageToken.getMessage();
    System.out.println("receive message from " + m);
    return MessageCallback.Action.CommitSuccess;
});
```

The value of accessKey is the AccessKeyID of your Alibaba Cloud account, and the value of accessSecret is the AccessKeySecret corresponding to the AccessKeyID. Log on to the [Alibaba Cloud console](https://home.console.aliyun.com/new#/), move the pointer to your account image, and click **AccessKey** to view your AccessKey ID and AccessKey Secret; click **Security Settings** to view your account ID.

The value of region is the region ID of your IoT Platform service.

## Configure the message receiving interface {#section_fnd_3x5_42b .section}

Once the connection is established, the server immediately pushes the subscribed messages to the SDK. Therefore, when you are configuring the connection, you configure the message-receiving interface, which is used to receive the messages for which callback has not been configured. We recommend that you call setMessageListener to configure callback before you `connect` the SDK to IoT Platform.

Use the `consume` method of MessageCallback interface and call the `setMessageListener()` of `messageClient` to configure the message receiving interface.

The returned result of `consume` determines whether the SDK sends an ACK.

The method of message receiving interface configuration is as follows:

```
MessageCallback messageCallback = new MessageCallback() {
    @Override
    public boolean consume(MessageToken messageToken) {
        Message m = messageToken.getMessage();
        log.info("receive : " + new String(messageToken.getMessage().getPayload()));
        return true;
    }
};
messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);
```

The parameters are introduced as follows:

-   MessageToken indicates the body of the returned message. Use `MessageToken.getMessage()` to get the message body. MessageToken is required when you reply to ACKs manually.

    A message body contains the following:

    ```
    public class Message {
        // Message body
        private byte[] payload;
        // Topic 
        private String topic;
        // Message ID
        private String messageId;
        // QoS
        private int qos;
    }
    ```

-   For more information about message body, see [message parameters](../../../../intl.en-US/Quick Start/Quick start for IoT Platform Basic/Servers subscribe to device messages.md#table_b2w_4nh_1fb).
-   `messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);` is a method to specify topics for callbacks.

    You can specify topics for callbacks, or you can use the generic callback.

    -   Callbacks with specified topics

        Callbacks with specified topics have higher priority than the generic callback. When a message matches with multiple topics, the callback with the topic whose elements ranks higher in the dictionary order is called and only one callback is performed.

        When you are configuring a callback, you can specify the topics with wildcards, for example, `/${YourProductKey}/${YourDeviceName}/#`.

        Example:

        ```
        messageClient.setMessageListener("/alEddfaXXXX/device1/#",messageCallback);
        //When the received message matches with the specified topic, for example, "/alEddfaXXXX/device1/update", the callback with this topic is called.
        ```

    -   Generic callback

        If you do not specify any topic for callbacks, generic callback is called.

        The method to configure the generic callback:

        ```
        messageClient.setMessageListener(messageCallback);
        //When the received message does not match with any specified topics which are configured for callbacks, the generic callback is called.
        ```

-   Configure ACK reply

    After a message with QOS\>0 is consumed, an ACK must be sent in reply. SDKs support sending ACKs as replies both automatically and manually. The default setting is to reply with ACKs automatically. In this example, no ACK reply setting is configured, so the system replies ACKs automatically.

    -   Reply ACKs automatically: If the returned value of `MessageCallback.consume` is true, the SDK will reply an ACK automatically; If the returned value is false or an exception occurs, the SDK will not reply any ACK. If no ACK is replied for the messages with QOS\>0, the server will send the message again.
    -   Reply ACKs manually: Use `MessageClient.setManualAcks`to configure for replying ACKs manually.

        Call `MessageClient.ack()` to reply ACKs manually, and the parameter MessageToken is required. You can obtain the value of MessageToken from the received message.

        The method to manually reply ACKs is as follows:

        ```
        messageClient.ack(messageToken);
        ```


## Demo {#section_bt1_hsh_1fb .section}

Click [SDK demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/http2-server-side-demo.zip) to download the demo.

