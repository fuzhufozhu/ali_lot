# Create a product \(Basic Edition\) {#task_q1k_mnl_vdb .task}

The first step when you start using IoT Platform is to create products. A product is a collection of devices that typically have the same features. For example, a product can refer to a product model and a device is then a specific device of the product model.

IoT Platform supports two editions of products: Basic Edition and Pro Edition. This article introduces how to create a Basic Edition product.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/). 
2.  In the left-side navigation pane, click **Devices** \> **Product**, and then click **Create Product**. 
3.  Select **Basic Edition** and click **Next**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12825/15445978706711_en-US.png)

4.  Enter required information and click **OK**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12825/154459787031991_en-US.png)

    The parameters are described as follows:

    |Parameter|Description|
    |:--------|:----------|
    |Product Name|The name of the product that you want to create. The product name must be unique within the account. For example, you can enter the product model as the product name. A product name is 4 to 30 characters in length, and can contain English letters, digits and underscores.|
    |Node Type|Options are Device and Gateway.    -   Device: Indicates that devices of this product cannot be mounted with sub-devices. This kind of devices can connect to IoT Platform directly or as sub-devices of gateway devices.
    -   Gateway: Indicates that devices of this product connect to IoT Platform directly and can be mounted with sub-devices . A gateway can manage sub-devices, maintain topological relationships with sub-devices, and synchronize topological relationships to IoT Platform.
For more information about gateway devices and sub-devices, see [Gateways and sub-devices](intl.en-US/User Guide/Create products and devices/Gateways and sub-devices/Gateways and sub-devices.md#).

|
    |Product Description|Describe the product information. You can enter up to 100 characters.|


After the product is created successfully, you are automatically redirected to the Products page. You can then view or edit the product information.

