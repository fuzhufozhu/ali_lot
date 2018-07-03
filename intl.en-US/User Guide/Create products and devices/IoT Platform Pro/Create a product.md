# Create a product {#task_lxd_pnl_vdb .task}

A product is a collection of devices, which typically have the same features.

This document describes how to create an IoT Platform Pro product in the console.

1.   Log on to the [IoT Platform console](http://iot.console.aliyun.com/) . 
2.   On the Products  page, click **Create Product**. 
3.   In the dialog box, select **Pro Edition**, enter a valid product name, choose a node type, device type, and data format, and then click **OK**. 

    ![](images/2362_en-US.jpg)

    Descriptions:

    -   Product Name: Enter a name for your product. The product name will act as a unique identifier. For example, you can enter the product model as the product name. 
    -   Node Type: either a device or gateway.
        -   Device: a device that cannot attach sub-devices. This kind of device can be connected directly to the IoT Hub, or as a sub-device of the gateway, which is connected to the IoT Hub.
        -   Gateway: A device that connects directly to the IoT Hub. Sub-devices can be attached to the gateway. The gateway can manage sub-devices, maintain the topological relationship of sub-devices, and synchronize the topological relationship to the cloud.
    -   Device Type: A standard template with predefined features. The Alibaba Cloud IoT Platform provides various templates with predefined features. For example, the Electric Meter template contains the following predefined features: power usage, voltage, electric current, total power consumption and other standard features. You can select a feature template, and edit the predefined features. You may also customize the template by adding additional user-defined features. If you select **None**, no standard templates is created, and you need to set user-defined features for the product.
    -   Data Format: The format of the uploaded or downloaded device data, you can select **Alink  JSON** or **Passthrough/Custom**.
        -   Alink JSON: A data exchange protocol between devices and the cloud. It is provided as JSON format in IoT Platform Pro for developers.
        -   Passthrough/Custom: If you want to customize the serial data format, you can choose **Passthrough/Custom**. You need to convert user-defined data to Alink JSON script, and configure the data parsing script in the cloud.

The product is created successfully. Next, define the product feature Thing Specification Language \(TSL\). For the methods and examples, see [Define Thing Specification Language models](intl.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#).

