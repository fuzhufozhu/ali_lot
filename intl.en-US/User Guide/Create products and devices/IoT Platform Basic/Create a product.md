# Create a product {#task_q1k_mnl_vdb .task}

A product is a collection of devices, which typically have the same features.

This document describes how to create a Basic Edition IoT product in the console.

1.   Log on to the [IoT Platform console](http://iot.console.aliyun.com/). 
2.   On the Products page, click **Create Product**. 
3.   In the dialog box that appears, select **Basic Edtion**, enter a valid product name, choose a node type, and then click **OK**. 

    ![](images/2238_en-US.jpg)

    Descriptions:

    -   Product Name: Enter a name for your product. The product name will act as a unique identifier. For example, you can enter the product model as the product name.
    -   Node Type: Either a device or gateway.
        -   Device: A device that cannot attach sub-devices. This kind of device can be connected directly to the IoT Hub, or attached as sub-device to the gateway that is connected to the IoT Hub.
        -   Gateway: A device that connects directly to the IoT Hub. Sub-devices can be attached to the gateway. The gateway can manage sub-devices, maintain the topological relationship of sub-devices, and synchronize the topological relationship to the cloud.

After the product is created successfully, you are automatically redirected to the Products page. You can view or edit the product information.

