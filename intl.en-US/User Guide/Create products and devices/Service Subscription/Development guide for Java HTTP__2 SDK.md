# Development guide for Java HTTP/2 SDK {#concept_cgg_k2v_y2b .concept}

This article introduces how to configure the service subscription, connect to the HTTP/2 SDK, authenticate identity, and configure the message-receiving interface.

Specifically, this section details the development process of the service subscription. Download the [server side Java HTTP/2 SDK demo](http://aliyun-iot.oss-cn-hangzhou.aliyuncs.com/java-http2-sdk-demo/http2-server-side-demo.zip).

## Configure service subscription {#section_tbd_2s5_42b .section}

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, click **Devices** \> **Product**.
3.  In the product list, find the product for which you want to configure the service subscription and click **View**. You are directed to the Product Details page.
4.  Click **Service Subscription** \> **Set Now**.
5.  Select the types of notifications that you want to push to the SDK.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18850/155497276412666_en-US.png)

    -   Device Upstream Notification: Indicates the messages of the topics to which devices are allowed to publish messages. If this notification type is selected, the HTTP/2 SDK can receive messages reported by devices.

        Devices report custom data and TSL data of properties, events, responses to property setting requests, and responses to service calling requests.

        For example, a product has three topic categories:

        -   `/${YourProductKey}/${YourDeviceName}/user/get`, devices can subscribe to messages.
        -   `/${YourProductKey}/${YourDeviceName}/user/update`, devices can publish messages.
        -   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, devices can publish messages.
        Service Subscription can push messages of the topics `/${YourProductKey}/${YourDeviceName}/user/update` and `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, to which devices can then publish messages. Additionally, the messages of `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post` are processed by the system before being pushed.

    -   Device Status Change Notification: Indicates the notifications that are sent when the statuses of devices change, for example, notifications for when devices go online or go offline. The topic `/as/mqtt/status/${YourProductKey}/${YourDeviceName}` has device status change messages. After this notification type is selected, the HTTP/2 SDK can receive the device status change notifications.
    -   Sub-Device Data Report Detected by Gateway: Gateways can report the information of sub-devices that are discovered locally. To use this feature, make sure that the applications on the gateway support this feature.
    -   Device Topological Relation Changes: Includes notifications about creation and removal of the topological relation between a gateway and its sub-devices.
    -   Device Changes Throughout Lifecycle: Includes notifications about device creation, deletion, disabling, and enabling.

**Note:** For messages of device properties, events, and services, Device Status Change Notification, Sub-Device Data Report Detected by Gateway, Device Topological Relation Changes, and Device Changes Throughout Lifecycle, the QoS is 0 by default. For other Device Upstream Notification messages \(except messages of device properties, events, and services\), you can set the OoS is 0 or 1 on your device SDK.

## Connect to the SDK {#section_v3d_gj5_42b .section}

Add the maven dependency to the project to connect to the SDK.

```
<dependency>
    <groupId>com.aliyun.openservices</groupId>
    <artifactId>iot-client-message</artifactId>
    <version>1.1.3</version>
</dependency>

<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-core</artifactId>
    <version>3.7.1</version>
</dependency>
```

## Identity authentication {#section_atv_yl5_42b .section}

Use the AccessKey information of your account for identity authentication and to build the connection between the SDK and IoT Platform.

Example:

```
//Your account accessKeyID
        String accessKey = "xxxxxxxxxxxxxxx";
        // Your account AccessKeySecret
        String accessSecret = "xxxxxxxxxxxxxxx";
        // regionId
        String regionId = "cn-shanghai";
        // Your account ID.
        String uid = "xxxxxxxxxxxx";
        // endPoint:  https://${uid}.iot-as-http2.${region}.aliyuncs.com
        String endPoint = "https://" + uid + ".iot-as-http2." + regionId + ".aliyuncs.com";

        // Connection configuration
        Profile profile = Profile.getAccessKeyProfile(endPoint, regionId, accessKey, accessSecret);

        // Construct the client
        MessageClient client = MessageClientFactory.messageClient(profile);

        // Receive data
        client.connect(messageToken -> {
            Message m = messageToken.getMessage();
            System.out.println("receive message from " + m);
            return MessageCallback.Action.CommitSuccess;
        });
```

The value of accessKey is the AccessKeyID of your account, and the value of accessSecret is the AccessKeySecret corresponding to the AccessKeyID. Log on to the [console](https://partners-intl.console.aliyun.com), hover the mouse over your account image, and click **AccessKey** to view your AccessKeyID and AccessKeySecret. You can also click **Security Settings** to view your account ID.

The value of regionId is the region ID of your IoT Platform service.

## Configure the message receiving interface {#section_fnd_3x5_42b .section}

Once the connection is established, the server immediately pushes the subscribed messages to the SDK. Therefore, when you are configuring the connection, you also configure the message-receiving interface, which is used to receive the messages for which callback has not been configured. We recommend that you call setMessageListener to configure a callback before you `connect` the SDK to IoT Platform.

Use the `consume` method of MessageCallback interface and call the `setMessageListener()` of `messageClient` to configure the message receiving interface.

The returned result of `consume` determines whether the SDK sends an ACK.

The method for configuring the message receiving interface is as follows:

```
MessageCallback messageCallback = new MessageCallback() {
    @Override
    public Action consume(MessageToken messageToken) {
        Message m = messageToken.getMessage();
        log.info("receive : " + new String(messageToken.getMessage().getPayload()));
        return MessageCallback.Action.CommitSuccess;
    }
};
messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);
```

The parameters are as follows:

-   MessageToken indicates the body of the returned message. Use `MessageToken.getMessage()` to get the message body. MessageToken is required when you send ACKs manually.

    A message body example is as follows:

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

-   For more information, see [Message body format](#).
-   `messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);` is a method to specify topics for callbacks.

    You can specify topics for callbacks, or you can use the generic callback.

    -   Callbacks with specified topics

        Callbacks with specified topics have higher priority than the generic callback. When a message matches with multiple topics, the callback with the topic whose elements rank higher in the lexicographical order is called and only one callback is performed.

        When you are configuring a callback, you can specify the topics with wildcards, for example, `/${YourProductKey}/${YourDeviceName}/#`.

        Example:

        ```
        messageClient.setMessageListener("/alEddfaXXXX/device1/#",messageCallback);
        //When the received message matches with the specified topic, for example, "/alEddfaXXXX/device1/update", the callback with this topic is called.
        ```

    -   Generic callback

        If you do not specify any topic for callbacks, the generic callback is called.

        The method for configuring the generic callback is as follows:

        ```
        messageClient.setMessageListener(messageCallback);
        //If the received message does not match with any specified topics which are configured for callbacks, the generic callback is called.
        ```

-   Configure ACK reply

    After a message with QOS\>0 is consumed, an ACK must be sent as the reply. SDKs support sending ACKs as replies both automatically and manually. The default setting is to reply with ACKs automatically. In this example, no ACK reply setting is configured, so the system replies with ACKs automatically.

    -   Reply ACKs automatically: If the returned value of `MessageCallback.consume` is true, the SDK will reply an ACK automatically; If the returned value is false or an exception occurs, the SDK will not reply with any ACK. If no ACK is replied for the messages with QOS\>0, the server will send the message again.
    -   Reply ACKs manually: Use `MessageClient.setManualAcks`to configure for replying ACKs manually.

        Call `MessageClient.ack()` to reply ACKs manually, and the parameter MessageToken is required. You can obtain the value of MessageToken from the received message.

        The method to manually reply ACKs is as follows:

        ```
        messageClient.ack(messageToken);
        ```


## Message body format {#section_csq_yv5_4fb .section}

-   Device status notification:

    ```
    {
        "status":"online|offline",
        "productKey":"12345565569",
        "deviceName":"deviceName1234",
        "time":"2018-08-31 15:32:28.205",
        "utcTime":"2018-08-31T07:32:28.205Z",
        "lastTime":"2018-08-31 15:32:28.195",
        "utcLastTime":"2018-08-31T07:32:28.195Z",
        "clientIp":"123.123.123.123"
    }
    ```

    |Parameter|Type|Description|
    |---------|----|-----------|
    |status|String|Device status: online or offline.|
    |productKey|String|The unique identifier of the product to which the device belongs.|
    |deviceName|String|The name of the device.|
    |time|String|The time when the notification is sent.|
    |utcTime|String|The UTC time when the notification is sent.|
    |lastTime|String|The time when the last communication occurred before this status change.|
    |utcLastTime|String|The UTC time when the last communication occurred before this status change.|
    |clientIp|String|The Internet IP address for the device.|

    **Note:** We recommend that you maintain your device status according to the value of the parameter lastTime.

-   Device lifecycle change:

    ```
    {
    "action" : "create|delete|enable|disable",
    "iotId" : "4z819VQHk6VSLmmBJfrf00107ee201",
    "productKey" : "12345565569",
    "deviceName" : "deviceName1234",
    "deviceSecret" : "",
    "messageCreateTime": 1510292739881
    }
    ```

    |Parameter|Type|Description|
    |---------|----|-----------|
    |action|String|     -   create: Create devices.
    -   delete: Delete devices.
    -   enable: Enable devices.
    -   disable: Disable devices.
 |
    |iotId|String|The unique identifier of the device within IoT Platform.|
    |productKey|String|The ProductKey of the product.|
    |deviceName|String|The name of the device.|
    |deviceSecret|String|The device secret. This parameter is included only when the value of action is create.|
    |messageCreateTime|Long|The timestamp when the message is generated, in milliseconds.|

-   Device topological relationship change:

    ```
    {
    "action" : "add|remove|enable|disable",
    "gwIotId": "4z819VQHk6VSLmmBJfrf00107ee200",
    "gwProductKey": "1234556554",
    "gwDeviceName": "deviceName1234",
    "devices": [
    {
    "iotId": "4z819VQHk6VSLmmBJfrf00107ee201",
    "productKey": "12345565569",
    "deviceName": "deviceName1234"
    }
    ],
    "messageCreateTime": 1510292739881
    }
    ```

    |Parameter|Type|Description|
    |---------|----|-----------|
    |action|String|     -   add: Add topological relationships.
    -   remove: Delete topological relationships.
    -   enable: Enable topological relationships.
    -   disable: Disable topological relationships.
 |
    |gwIotId|String|The unique identifier of the gateway device.|
    |gwProductKey|String|The ProductKey of the product to which the gateway device belongs.|
    |gwDeviceName|String|The name of the gateway device.|
    |devices|Object|The sub-devices whose topological relationship with the gateway will be changed.|
    |iotId|String|The unique identifier of the sub-device.|
    |productKey|String|The ProductKey of the product to which the sub-device belongs.|
    |deviceName|String|The name of the sub-device.|
    |messageCreateTime|Long|The timestamp when the messages is generated, in milliseconds.|

-   A gateway detects and reports sub-devices:

    ```
    {
        "gwIotId":"4z819VQHk6VSLmmBJfrf00107ee200",
        "gwProductKey":"1234556554",
        "gwDeviceName":"deviceName1234",
        "devices":[
            {
                "iotId":"4z819VQHk6VSLmmBJfrf00107ee201",
                "productKey":"12345565569",
                "deviceName":"deviceName1234"
            }
        ]
    }
    ```

    |Parameter|Type|Description|
    |---------|----|-----------|
    |gwIotId|String|The unique identifier of the gateway device.|
    |gwProductKey|String|The unique identifier of the gateway product.|
    |gwDeviceName|String|The name of the gateway device.|
    |devices|Object|The sub-devices detected by the gateway.|
    |iotId|String|The unique identifier of the sub-device.|
    |productKey|String|The ProductKey of the product that the sub-device belongs to.|
    |deviceName|String|The name of the sub-device.|


