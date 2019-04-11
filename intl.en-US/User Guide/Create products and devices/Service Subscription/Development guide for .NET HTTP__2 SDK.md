# Development guide for .NET HTTP/2 SDK {#concept_cgg_k2v_y2b .concept}

This topic describes how to configure service subscription, connect a .NET SDK of the HTTP/2 server to IoT Platform, perform identity authentication, and set the message receiving interface.

The following process describes how to develop service subscription. Download the [server side .NET SDK demo](https://iot-demos.oss-cn-shanghai.aliyuncs.com/h2/iot-http2-net-sdk-demo.zip).

## Configure service subscription {#section_tbd_2s5_42b .section}

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  From the left-side navigation pane, choose **Devices** \> **Product**.
3.  In the product list, locate the product for which you want to configure service subscription and click **View**. The Product Details page appears.
4.  Click **Service Subscription** \> **Set**.
5.  Select the types of notifications that you want to push.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18850/155496720912666_en-US.png)

    -   Device Upstream Notification: Indicates the messages in the topics to which devices are allowed to publish messages. If this notification type is selected, the HTTP/2 SDK can receive messages reported by devices.

        Devices can report both custom data and TSL data of properties, events, responses to property settings, and responses to service callings .

        For example, a product has three topic categories:

        -   `/${YourProductKey}/${YourDeviceName}/user/get`, to which devices can subscribe.
        -   `/${YourProductKey}/${YourDeviceName}/user/update`, to which devices can publish messages.
        -   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, to which devices can publish messages.
        Service subscription can push messages in the topics `/${YourProductKey}/${YourDeviceName}/user/update` and `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`, to which devices can publish messages. Additionally, the messages in the topics `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post` are processed by the system.

    -   Device Status Change Notification: Indicates the notifications that are sent when the statuses of devices change, for example, the device connection and device disconnection notifications. The topic which is used to send device status change notifications: `/as/mqtt/status/${YourProductKey}/${YourDeviceName}`. After this notification type is selected, the HTTP/2 SDK can receive the device status change notifications.
    -   Sub-Device Data Report Detected by Gateway: A gateway can report the information about sub-devices that are discovered locally. Make sure that the gateway has an application that can discover and report sub-device information.
    -   Device Topological Relation Changes: It includes notifications about device topological relation change.
    -   Device Changes Throughout Lifecycle: It includes notifications about device creation, deletion, disabling, and enabling.

**Note:** For messages of device properties, events, and services, Device Status Change Notification, Sub-Device Data Report Detected by Gateway, Device Topological Relation Changes, and Device Changes Throughout Lifecycle, the QoS is 0 by default. For other Device Upstream Notification messages \(except messages of device properties, events, and services\), you can set the OoS is 0 or 1 on your device SDK.

## Connect the HTTP/2 SDK to IoT Platform {#section_v3d_gj5_42b .section}

Add dependency package [iotx-as-http2-net-sdk.dll](https://iot-demos.oss-cn-shanghai.aliyuncs.com/h2/iotx-as-http2-net-sdk.dll) to a project.

## Authenticate identity {#section_atv_yl5_42b .section}

To use the service subscription feature, you must use the AccessKey information of your account for identity authentication and establish a connection between the HTTP/2 SDK and IoT Platform.

Example:

```
//The AccessKey ID of your Alibaba Cloud account
string accessKey = "xxxxxxxxxxxxxxx";
//The AccessKey Secret of your Alibaba Cloud account
string accessSecret = "xxxxxxxxxxxxxxx";
//The region ID of your IoT Platform service
string regionId = "cn-shanghai";
//The UID of your Alibaba Cloud account
string uid = "xxxxxxxxxxxxxxx";
//The domain name
string domain = ".aliyuncs.com";
//The endpoint
string endpoint = "https://" + uid + ".iot-as-http2." + regionId + domain;

//Configure connection parameters
Profile profile = new Profile();
profile.AccessKey = accessKey;
profile.AccessSecret = accessSecret;
profile.RegionId = regionId;
profile.Domain = domain;
profile.Url = endpoint;
//Clear accumulated messages
profile.CleanSession = true;
profile.GetAccessKeyAuthParams();

//Construct the client
IMessageClient client = new MessageClient(profile);

//Connect to the HTTP/2 server to receive messages
client.DoConnection(new DefaultHttp2MessageCallback());

//Configure a specified topic for callback 
client.SetMessageListener("/${YourProductKey}/#", new CustomHttp2MessageCallback());
```

The value of accessKey is the AccessKey ID of your account, and the value of accessSecret is the AccessKey Secret corresponding to the AccessKey ID. Log on to the [Official console](https://partners-intl.console.aliyun.com), hover over your account avatar, and click **AccessKey** to view your AccessKey ID and AccessKey Secret. You can also click **Security Settings** to view your account ID.

The value of regionId is the region ID of your IoT Platform service.

## Configure the message receiving interface {#section_fnd_3x5_42b .section}

After the connection is established, IoT Platform immediately pushes the subscribed messages to the HTTP/2 SDK. Therefore, you must configure the message receiving interface.

The message receiving interface is as follows:

```
public interface IHttp2MessageCallback
{
    ConsumeAction Consume(Http2ConsumeMessage http2ConsumeMessage);
}
```

You must use the `consume` method of IHttp2MessageCallback to set the message receiving interface.

Configure the message receiving interface as follows:

```
public class DefaultHttp2MessageCallback : IHttp2MessageCallback
    {
        public DefaultHttp2MessageCallback()
        {
        }

        public ConsumeAction Consume(Http2ConsumeMessage http2ConsumeMessage)
        {
            Console.WriteLine("receive : " + http2ConsumeMessage.MessageId);
            //Automatically return an ACK
            return ConsumeAction.CommitSuccess;
        }
    }
```

The parameters are as follows:

-   Http2ConsumeMessage indicates the body of the returned message.

    A message body contains the following information:

    ```
    public class Http2ConsumeMessage
    {
        //The message body
        public byte[] Payload { get; set; }
        //The topic
        public string Topic { get; set; }
        //The message ID
        public string MessageId { get; set; }
        //QoS
        public int Qos { get; set; }
        //The connection parameter
        public Http2Connection Connection { get; set; }
    }
    ```

-   For more information, see [Message body format](#).
-   `messageClient.setMessageListener("/${YourProductKey}/#",messageCallback);` is a method to configure a callback. In this example, a topic is configured for the callback.

    You can specify topics for callbacks, or you can use the generic callback.

    -   Topic-specific callbacks

        A topic-specific callback has higher priority than the generic callback. When a message matches multiple topics, the callback with the topic whose elements rank higher in the lexicographical order is called and only one callback is performed.

        When you configure a callback, you can specify a topic with wildcards, for example, `/${YourProductKey}/${YourDeviceName}/#`.

        Example:

        ```
        client.SetMessageListener("/alEddfaXXXX/device1/#",messageCallback);
        //When the received message matches the specified topic, for example, "/alEddfaXXXX/device1/update", the callback with this topic is called.
        ```

    -   Generic callback

        If you do not specify any topic for a callback, the generic callback is called.

        Configure the generic callback as follows:

        ```
        new DefaultHttp2MessageCallback()
        ```

-   Configure ACK replies.

    After a message with QoS\>0 is consumed, an ACK must be sent as the reply. The HTTP/2 SDK supports sending ACKs as replies both automatically and manually. By default, ACKs are returned automatically. In this example, no ACK reply setting is configured, so the system returns ACKs automatically.

    -   Return ACKs automatically: If the returned value of `IHttp2MessageCallback.consume` is ConsumeAction.CommitSuccess, the HTTP/2 SDK will return an ACK automatically. If the returned value is ConsumeAction.CommitFailure or an exception occurs, the HTTP/2 SDK will not return any ACK. If no ACK is returned for a message with QoS\>0, IoT Platform will send the message again.
    -   Return ACKs manually: Use `ConsumeAction.CommitFailure` to manually return an ACK.

        Call the `MessageClient.DoAck()` method to return ACKs manually. The method contains the following parameters: topic, messageId, and the connection parameter. You can obtain these parameter values from the received message.

        Manually return an ACK as follows:

        ```
        client.DoAck(connection, topic, messageId, delegate);
        ```


## Message body format {#messagebody .section}

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

    |Parameter|Data type|Description|
    |---------|---------|-----------|
    |status|String|The device status: online or offline.|
    |productKey|String|The unique identifier of the product to which the device belongs.|
    |deviceName|String|The name of the device.|
    |time|String|The time when the notification was sent.|
    |utcTime|String|The UTC time when the notification was sent.|
    |lastTime|String|The time when the last communication occurred before this status change.|
    |utcLastTime|String|The UTC time when the last communication occurred before this status change.|
    |clientIp|String|The public outbound IP address of the device.|

    **Note:** We recommend that you maintain your device status according to the value of the lastTime parameter.

-   Device lifecycle change notification:

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

    |Parameter|Data type|Description|
    |---------|---------|-----------|
    |action|String|     -   create: Create a device.
    -   delete: Delete a device.
    -   enable: Enable a device.
    -   disable: Disable a device.
 |
    |iotId|String|The unique identifier of the device in IoT Platform.|
    |productKey|String|The unique identifier of the product to which the device belongs.|
    |deviceName|String|The name of the device.|
    |deviceSecret|String|The device key. This parameter is included only when the value of action is create.|
    |messageCreateTime|Long|The timestamp when the message was generated, in milliseconds.|

-   Device topological relationship change notification:

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

    |Parameter|Data type|Description|
    |---------|---------|-----------|
    |action|String|     -   add: Build topological relationships.
    -   remove: Delete topological relationships.
    -   enable: Enable topological relationships.
    -   disable: Disable topological relationships.
 |
    |gwIotId|String|The unique identifier of the gateway device.|
    |gwProductKey|String|The unique identifier of the product to which the gateway device belongs.|
    |gwDeviceName|String|The name of the gateway device.|
    |devices|Object|The sub-devices whose topological relationships with the gateway will be changed.|
    |iotId|String|The unique identifier of the sub-device.|
    |productKey|String|The unique identifier of the product to which the sub-device belongs.|
    |deviceName|String|The name of the sub-device.|
    |messageCreateTime|Long|The timestamp when the message was generated, in milliseconds.|

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

    |Parameter|Data type|Description|
    |---------|---------|-----------|
    |gwIotId|String|The unique identifier of the gateway device.|
    |gwProductKey|String|The unique identifier of the product to which the gateway device belongs.|
    |gwDeviceName|String|The name of the gateway device.|
    |devices|Object|The sub-devices discovered by the gateway.|
    |iotId|String|The unique identifier of the sub-device.|
    |productKey|String|The unique identifier of the product to which the sub-device belongs.|
    |deviceName|String|The name of the sub-device.|


