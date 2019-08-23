# Use the rules engine to establish M2M communication {#task_y43_gh1_ydb .task}

This topic describes how to use the data forwarding feature of the rules engine in IoT Platform to build an M2M communication architecture. A connection between a smart lamp and a mobile app is used as an example.

The following figure shows the procedure to control the smart lamp through the mobile app.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13908/15665290314208_en-US.PNG)

1.  In the IoT Platform console, create a product and device for the smart lamp and define the features of the product. For more information, see [Create a product](../../../../reseller.en-US/User Guide/Create products and devices/Create a product.md#), [Create multiple devices at a time](../../../../reseller.en-US/User Guide/Create products and devices/Create devices/Create multiple devices at a time.md#), and [Define features](../../../../reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#). In this example, the ProductKey and DeviceName parameters for the smart lamp are set to al123456789 and light respectively.
2.  Develop a device SDK for the smart lamp. In this example, the device and IoT Platform use MQTT to communicate with each other.
3.  In the IoT Platform console, register a product and device for the mobile app. In this example, the ProductKey and DeviceName parameters for the mobile app are set to al987654321 and ControlApp respectively. When a user registers the mobile app with your server, your server sends the device information to the mobile app. This way, the mobile app can connect to IoT Platform as a device.
4.  Develop the mobile app. In this example, the mobile app and IoT Platform use HTTPS to communicate with each other.
5.  Configure a data forwarding rule to forward commands from the mobile app to the topic of the smart lamp. 
    1.  Log on to the IoT Platform console. In the left-side navigation pane, click Rules. On the Rules page, create a data forwarding rule.
    2.  Write an SQL statement to process message contents to be forwarded. This SQL statement retrieves the messages to be sent from the mobile app to the smart lamp from the topic of the app device. 

        In this example, the SQL statement retrieves the deviceName, timestamp, and switch fields of the app device.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13908/156652903247216_en-US.png)

    3.  Configure the destination to which you want to forward processed data. Subscribe the smart lamp to the device topic to receive commands from the app. 

        **Note:** 

        -   Select **Publish to Another Topic**.
        -   When specifying a device, use `${deviceName}` to match all target smart lamp devices. In this example, `${deviceName}` is converted to the name of the smart lamp device. The name is `light`.
        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13908/156652903247218_en-US.png)

6.  The user scans the QR code to bind the app to the smart lamp. After the app sends a request to the server to bind a device, the server binds the smart lamp and returns the device name. The returned name is specified by the deviceName parameter. In this example, the smart lamp device is named light.
7.  The user sends control commands through the app. 

    1.  The app sends commands to the topic in IoT Platform. In this example, the topic is `/al987654321/ControlApp/user/update`. Only JSON format commands are supported.

    2.  IoT Platform then sends the commands to the topic of the smart lamp based on the user-defined data forwarding rule. In this example, the topic defined is `/al123456789/light/user/set`.
    3.  After receiving the commands, the smart lamp device performs the corresponding operations.
    **Note:** The app can also send a request to your server to unbind the smart lamp device. After the device is unbound, the app no longer controls the smart lamp.


