# Create a product \(Pro Edition\) {#task_lxd_pnl_vdb .task}

The first step in using IoT Platform is to create products. A product is a collection of devices that typically have the same features. For example, a product can refer to a product model and a device is then a specific device of the product model.

IoT Platform supports two editions of products: Basic Edition and Pro Edition. This article introduces how to create Pro Edition products in the IoT Platform console.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/). 
2.  In the left-side navigation pane, click Products and then click **Create Product**. 
3.  Select **Pro Edition** as the version, enter all the required information, and then click **OK**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12827/15404599082362_en-US.jpg)

    The parameters are described as follows:

    |Parameter|Description|
    |:--------|:----------|
    |Product Name|The name of the product that you want to create. The product name must be unique within the Alibaba Cloud account. For example, you can enter the product model as the product name.|
    |Node Type|     -   Device: A device that cannot attach sub-devices. This type of device can be connected directly to the IoT Hub, or attached as sub-device to a gateway.
    -   Gateway: A device that connects directly to the IoT Hub. Sub-devices can be attached to a gateway. A gateway can manage sub-devices, maintain the topological relationship with sub-devices, and synchronize the topological relationship to the cloud.
 For more information about gateway devices and sub-devices, see [Gateways and sub-devices](reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices/Gateways and sub-devices.md#).

 |
    |Device Type|Select a standard template with predefined features. If you select **None**, no standard template is created and you must manually define features for the product.**Note:** 

Alibaba Cloud IoT Platform provides various templates with predefined features. For example, the **Electric Meter** template contains the following predefined features: power usage, voltage, electric current, total power consumption and other standard features. You can also customize the template by editing the features or adding additional user-defined features.

|
    |Data Type|The format of the uploaded or downloaded device data. You can select either **Alink JSON** or **Do not parse/Custom**.    -   Alink JSON: A data exchange protocol between devices and the IoT Hub. It is provided in JSON format.
    -   Do not parse/Custom: If you want to customize the serial data format, select **Do not parse/Custom**. You must then convert the user-defined formatted data to Alink JSON script by using data parsing function so that your devices can communicate with the IoT Hub.
|
    |Connect to Gateway**Note:** Required when the node type is **Device**.

|Will this product connect to a gateway product as its sub-device?    -   **Yes**: This product can be connect to a gateway.

Select a protocol used for sub-device and gateway communication.

        -   Modbus: Indicates the connection protocol for sub-device and gateway communication is Modbus.
        -   OPC UA: Indicates the connection protocol for sub-device and gateway communication is OPC UA.
        -   Custom: Indicates that you want to use another protocol as the connection protocol for sub-device and gateway communication.
    -   **No**: This product cannot be connect to a gateway.
|
    |Product Description|Describe the product information.|

4.  Click **OK**. 

    After the product is created successfully, you are automatically redirected to the Products page.


1.  To configure a product's features \(such as [Notifications](reseller.en-US/User Guide/Create products and devices/Topics/What is a topic?.md#), [TSL \(Define Feature\)](reseller.en-US/User Guide/Create products and devices/TSL/What is Thing Specification Language (TSL)?.md#), and [Service Subscription](reseller.en-US/User Guide/Create products and devices/Service Subscription/What is Service Subscription?.md#)\), go to the product list, find the target product and then click its corresponding **View** button
2.  To learn more about configurations you can apply during device development, see [Developer Guide \(Devices\)](../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).
3.  To publish a product, go to the product details page and click **Publish**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12827/154045990813396_en-US.png)

    Note that before you publish a product make sure that you have configured all the correct information for the product, have completed debugging of it, and have verified that it meets the criteria for being published.

    When the product status is **Published**, you can view the product information but cannot modify or delete the product.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12827/154045990813395_en-US.png)

    To cancel the publishing of a product, click **Cancel Publishing**.


