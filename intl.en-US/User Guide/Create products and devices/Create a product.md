# Create a product {#task_lxd_pnl_vdb .task}

The first step when you start using IoT Platform is to create products. A product is a collection of devices that typically have the same features. For example, a product can refer to a product model and a device is then a specific device of the product model.

This topic describes how to create products in the IoT Platform console.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  In the left-side navigation pane, click **Devices** \> **Product**, and then click **Create Product**. 
3.  Enter all the required information and then click **OK**. 

    The parameters are as follows.

    |Parameter|Description|
    |:--------|:----------|
    |Product Name|The name of the product that you want to create. The product name must be unique within the account. For example, you can enter the product model as the product name. A product name is 4 to 30 characters in length, and can contain Chinese characters, English letters, digits, and underscores. A Chinese character counts as two characters.|
    |Node Type|     -   **Device**: Indicates that devices of this product cannot be mounted with sub-devices. This kind of device can connect to IoT Platform directly or as a sub-device of a gateway device.
    -   **Gateway**: Indicates that devices of this product connect to IoT Platform directly and can be mounted with sub-devices . A gateway can manage sub-devices, maintain topological relationships with sub-devices, and synchronize topological relationships to IoT Platform.
 For more information about gateway devices and sub-devices, see [Gateways and sub-devices](reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices/Gateways and sub-devices.md#).

 |
    |Connect to Gateway**Note:** This parameter appears if the node type is **Device**.

|Indicates whether or not devices of this product can be connected to gateways as sub-devices.    -   **Yes**: Devices of this product can be connected to a gateway. If you select Yes here, you are required to select a gateway connection protocol under **Network Connection and Data**.
    -   **No**: Devices of this product cannot be connected to a gateway. If you select No here, you are required to select a network connection method under **Network Connection and Data**.
|
    |Gateway Connection Protocol**Note:** This parameter appears if you select **Yes** for **Connect to Gateway** .

|Select a protocol for sub-device and gateway communication.    -   Custom: Indicates that you want to use another protocol as the connection protocol for sub-device and gateway communication.
    -   Modbus: Indicates that the communication protocol between sub-devices and gateways is Modbus.
    -   OPC UA: Indicates that the communication protocol between sub-devices and gateways is OPC UA.
    -   ZigBee: indicates that the communication protocol between sub-devices and gateways is ZigBee.
    -   BLE: indicates that the communication protocol between sub-devices and gateways is BLE.
|
    |Network Connection Method**Note:** This parameter appears if you select **No** for **Connect to Gateway**.

|Select a network connection method for the devices:    -   WiFi
    -   Cellular \(2g/3g/4G\)
    -   Ethernet
    -   Other
|
    |Data Type|Select a format in which devices exchange data with IoT Platform. Options are **ICA Standard Data Format \(Alink JSON\)** and **Do not parse/Custom**.    -   ICA Standard Data Format \(Alink JSON\): The standard data format defined by IoT Platform for device and IoT Platform communication.
    -   Do not parse/Custom: If you want to customize the serial data format, select **Do not parse/Custom**. Custom formatted data must be converted to Alink JSON script by [Data parsing](reseller.en-US/User Guide/Create products and devices/Data parsing.md#)so that your devices can communicate with the IoT Platform.
|
    |Product Description|Describe the product information. You can enter up to 100 characters.|

    After the product is created successfully, you are automatically redirected to the Products page.


1.  To configure features for a product \(such as [Notifications](reseller.en-US/User Guide/Create products and devices/Topics/What is a topic?.md#), [TSL \(Define Feature\)](reseller.en-US/User Guide/Create products and devices/TSL/Overview.md#), and [Service Subscription](reseller.en-US/User Guide/Create products and devices/Service Subscription/What is Service Subscription?.md#)\), go to the product list, find the target product and then click its corresponding **View** button.
2.  Register devices on IoT Platform.
3.  Develop your physical devices by referring to [Developer Guide \(Devices\)](../../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).
4.  To publish a product, go to the product details page and click **Publish**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12827/155503582713396_en-US.png)

    Note that before you publish a product, you must make sure that you have configured all the correct information for the product, have completed debugging the features, and have verified that it meets the criteria for being published.

    When the product status is **Published**, you can view the product information but cannot modify or delete the product.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12827/155503582713395_en-US.png)

    To cancel the publishing of a product, click **Cancel Publishing**.


