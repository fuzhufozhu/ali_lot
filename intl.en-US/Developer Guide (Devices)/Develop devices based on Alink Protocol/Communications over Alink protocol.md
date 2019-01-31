# Communications over Alink protocol {#concept_pfw_hdg_cfb .concept}

IoT Platform provides device SDKs for you to configure devices. These device SDKs already encapsulate protocols for data exchange between devices and IoT Platform. You can use these SDKs to develop your devices. If these SDKs do not meet your business requirements, you can develop your own SDK with an Alink communication channel by yourself.

For SDKs provided by IoT Platform, see [Device SDKs](reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).

The Alink protocol is a data exchange standard for IoT development that allows communication between devices and IoT Platform. The protocol exchanges data that is formatted in Alink JSON.

The following sections describe the device connection procedures and data communication processes \(upstream and downstream\) when using the Alink protocol.

## Connect devices to IoT Platform {#section_dqf_tzg_cfb .section}

As shown in the following figure, devices can be connected to IoT Platform as directly connected devices or sub-devices. The connection process involves the following key steps: authenticate the device, establish a connection, and the device reports data to IoT Platform.

Directly connected devices can be connected to IoT Platform by using the following methods:

-   If [Unique-certificate-per-device authentication](reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-device authentication.md#) is enabled, install the device certificate \(ProductKey, DeviceName, and DeviceSecret\) to the physical device for authentication, connect the device to IoT Platform, and then report data to IoT Platform.
-   If dynamic registration based on [Unique-certificate-per-product authentication](reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-product authentication.md#) is enabled, install the product certificate \(ProductKey and ProductSecret\) to the physical device for authentication, connect the device to IoT Platform, and then report data to IoT Platform.

Sub-devices connect to IoT Platform through their gateways. Sub-devices can be connected to IoT Platform by using the following methods:

-   If [Unique-certificate-per-device authentication](reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-device authentication.md#) is enabled, install the ProductKey, DeviceName, and DeviceSecret to the physical sub-device for authentication. The sub-device then sends its certificate information to the gateway, and then the gateway builds the topological relationship.The sub-device data are sent to IoT Platform through the gateway communication channel.
-   If dynamic registration is enabled, install the ProductKey to the physical sub-device for authentication in advance. The sub-device sends the ProductKey and DeviceName to the gateway, and then the gateway forwards the ProductKey and DeviceName to IoT Platform. IoT Platform then verifies the received DeviceName and sends the DeviceSecret to the sub-device. The sub-device sends its certificate \(ProductKey, DeviceName, and DeviceSecret\) to the gateway for building topological relationship. The sub-device data are sent to IoT Platform through the gateway communication channel.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154892894111778_en-US.jpg)

## Devices report properties or events {#section_isq_tzg_cfb .section}

-   Pass-through \(Do not parse/Custom\) data

    ![](images/11785_en-US.jpeg)

    1.  The device reports raw data to IoT Platform using the topic for passing through data.
    2.  IoT Platform parses the received data using the data parsing script that you have submitted in the IoT Platform console. The `rawDataToProtocol` method in the script is called to convert the raw data reported by the device to Alink JSON data.
    3.  IoT Platform uses the Alink JSON data for further processes.

        **Note:** If you have configured rules for data forwarding, the Alink JSON data will be forwarded to the targets according to the rules.

        -   The data forwarded by the rules engine are the data that have been parsed by the data parsing script.
        -   When you configure SQL statements for rules, to obtain the device properties, specify the data topic to be `/sys/{productKey}/{deviceName}/thing/event/property/post`.
        -   When you configure SQL statements for rules, to obtain the device events, specify the data topic to be `/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post`.
    4.  IoT Platform calls the `protocolToRawData` method in the data parsing script to convert the result data to the data format of the device.
    5.  IoT Platform pushes the converted data to the device.
    6.  You can query the device property data using the API [QueryDevicePropertyData](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDevicePropertyData.md#) and query the device event data using the API [QueryDeviceEventData](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceEventData.md#).
-   Non-pass through \(Alink JSON\) data

    ![](images/11792_en-US.jpeg)

    1.  The device reports Alink JSON data to IoT Platform using the topic for non-pass through data.
    2.  IoT Platform handles the received data.

        **Note:** If you have configured rules for data forwarding, the data will be forwarded to the targets according to the rules.

        -   When you configure SQL statements for rules, to obtain the device properties, specify the data topic to be `/sys/{productKey}/{deviceName}/thing/event/property/post`.
        -   When you configure SQL statements for rules, to obtain the device events, specify the data topic to be `/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post`.
    3.  IoT Platform returns the results to the device.
    4.  You can query the device property data using the API [QueryDevicePropertyData](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDevicePropertyData.md#) and query the device event data using the API [QueryDeviceEventData](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceEventData.md#).

## Call device services or set device properties {#section_qzw_5dt_cfb .section}

-   Call device services or set device properties asynchronously

    ![](images/11793_en-US.jpeg)

    1.  Set a device property or call a device service using the asynchronous method.

        **Note:** 

        -   Call the API [SetDeviceProperty](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/SetDeviceProperty.md#) to set a property asynchronously.
        -   Call the API [InvokeThingService](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/InvokeThingService.md#) to call a service asynchronously \(if you select Asynchronous as the method when you define the service, this service is called in the asynchronous method\).
    2.  IoT Platform verifies the parameters.
    3.  IoT Platform uses the asynchronous method to handle the request and return the results. If the call is successful, the message ID is included in the response.

        **Note:** If the data type is pass-through \(Do not parse/Custom\), IoT Platform will call the `protocolToRawData` method in the data parsing script to convert the data before sending the data to the device.

    4.  IoT Platform sends the data to the device, and then the device handles the request asynchronously.

        **Note:** 

        -   If the data is pass-through \(Do not parse/Custom\) data, the topic for pass-through data is used.
        -   If the data is non-pass through \(Alink JSON\) data, the topic for non-pass through data is used.
    5.  After the device has completed the requested operation, it returns the results to IoT Platform.
    6.  IoT Platform receives the results, and
        -   If the data type is pass-through \(Do not parse/Custom\), IoT Platform will call the `rawDataToProtocol` method in the data parsing script to convert the data returned by the device.
        -   If you have configured rules for data forwarding, IoT Platform rules engine will forward the data to the targets according to the rules.
            -   When you configure SQL statements for rules, to obtain the results of service processing, specify the data topic as `/sys/{productKey}/{deviceName}/thing/downlink/reply/message`.
            -   If the data type is pass-through \(Do not parse/Custom\), the data forwarded by the rules engine is the data that has been parsed by the data parsing script.
-   Call services using the synchronous method.

    ![](images/11794_en-US.jpeg)

    1.  Call the API [InvokeThingService](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/InvokeThingService.md#) to call a service synchronously \( if you select Synchronous as the method when you define the service, this service is called in the synchronous method\).
    2.  IoT Platform verifies the parameters.
    3.  The synchronous call method is where IoT Platform calls the RRPC topic to send the request data to the device, and waits for the device to return a result.

        **Note:** If the data type of the device is Do not parse/Custom, IoT Platform will call the `protocolToRawData` method in the data parsing script to convert the data before sending the data to the device.

    4.  After the device has completed the requested operation, it returns the results to IoT Platform. If IoT Platform does not receive a result within the timeout period, it will send a timeout error to you.
    5.  IoT Platform returns the results to you.

        **Note:** If the data type of the device is Do not parse/Custom, IoT Platform will call the `rawDataToProtocol` method in the data parsing script to convert the data returned by the device, and then will send the results to you.


## Build topological relationships between gateways and sub-devices. {#section_dgr_xxw_4fb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154892894114317_en-US.png)

1.  After a sub-device has been connected to a gateway, the gateway sends a message using the topic for adding topological relationship messages to notify IoT Platform to build topological relationship between the gateway and the sub-device. IoT Platform handles the request and then returns a result.
2.  Also, a gateway can send a message using the topic for deleting topological relationship messages to notify IoT Platform to remove a sub-device from the gateway.
3.  Call the API [GetThingTopo](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/GetThingTopo.md#) to query topological relationships of devices.
4.  If you use the rules engine to forward device messages to another Alibaba Cloud service, and you receive device messages from that service, the process of building a topological relationship is as the following.
    1.  The gateway device reports the information of the sub-device that has been detected to IoT Platform.
    2.  IoT Platform receives the message and then forwards the message to the data forwarding target that you have specified when you were configuring the rule.
    3.  You obtain the sub-device information from the data forwarding target service and then determine whether or not to build the topological relationship. Call the API [NotifyAddThingTopo](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/NotifyAddThingTopo.md#) to send a request for building topological relationship to IoT Platform.
    4.  IoT Platform receives the request from [NotifyAddThingTopo](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/NotifyAddThingTopo.md#), and then pushes the request to the gateway.
    5.  The gateway receives the request and builds the topological relationship with the sub-device.

**Note:** 

-   Gateways use the topic `/sys/{productKey}/{deviceName}/thing/topo/add` to build topological relationships with sub-devices.
-   Gateways use the topic `/sys/{productKey}/{deviceName}/thing/topo/delete` to delete topological relationships with sub-devices.
-   Gateways use the topic `/sys/{productKey}/{deviceName}/thing/topo/get` to query the topological relationships with sub-devices.
-   Gateways use the topic `/sys/{productKey}/{deviceName}/thing/list/found` to report information of sub-devices.
-   Gateways use the topic `/sys/{productKey}/{deviceName}/thing/topo/add/notify` to initiate requests for building topological relationships.

