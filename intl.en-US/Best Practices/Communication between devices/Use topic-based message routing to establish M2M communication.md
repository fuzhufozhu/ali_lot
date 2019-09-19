# Use topic-based message routing to establish M2M communication {#task_q45_glr_ydb .task}

This topic describes how to use the topic-based message routing service of IoT Platform to build an M2M communication architecture. A connection between a smart lamp and a mobile app is used as an example.

The following figure shows the procedure to control the smart lamp through the mobile app.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13912/15688949914210_en-US.png)

1.  In the IoT Platform console, create a product and device for the smart lamp and define the features of the product. For more information, see [Create a product](../../../../reseller.en-US/User Guide/Create products and devices/Create a product.md#), [Create multiple devices at a time](../../../../reseller.en-US/User Guide/Create products and devices/Create devices/Create multiple devices at a time.md#), and [Define features](../../../../reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#). In this example, the ProductKey and DeviceName parameters for the smart lamp are set to al123456789 and light respectively.
2.  Develop a device SDK for the smart lamp. In this example, the device and IoT Platform use MQTT to communicate with each other.
3.  In the IoT Platform console, register a product and device for the mobile app. In the preceding figure, the ProductKey and DeviceName parameters for the mobile app are set to al987654321 and ControlApp respectively. When a user registers the mobile app with your server, your server sends the device information to the mobile app. This way, the mobile app can connect to IoT Platform as a device.
4.  Call the [CreateTopicRouteTable](../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage Topics/CreateTopicRouteTable.md#) operation to create a message routing relationship between the app topic and the smart lamp topic. 
    -   Set the SrcTopic parameter to the topic of the app: `/al987654321/ControlApp/user/update`.
    -   Set the DstTopics parameter to the topic of the smart lamp: `/al123456789/light/user/set`.
5.  Develop the mobile app. In this example, the mobile app and IoT Platform use HTTPS to communicate with each other.
6.  The user scans the QR code to bind the app to the smart lamp. After the app sends a request to the server to bind a device, the server binds the smart lamp and returns the device name. The returned name is specified by the deviceName parameter. In this example, the smart lamp device is named light.
7.  The user sends control commands through the app. 

    1.  The topic to which the app sends commands is `/al987654321/ControlApp/user/update`. Only JSON format commands are supported.

    2.  IoT Platform routes commands to the topic of the smart lamp device based on the defined message routing relationship. The topic is `/al123456789/light/user/set`.
    3.  After receiving the commands, the smart lamp device performs the corresponding operations.
    **Note:** You can configure the app to notify your server to delete the message routing relationship and your server calls the [DeleteTopicRouteTable](../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage Topics/DeleteTopicRouteTable.md#) operation to delete the message routing relationship. After the routing relationship is deleted, the app no longer controls the smart lamp.


