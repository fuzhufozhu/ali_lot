# Create a product \(Basic Edition\) {#task_q1k_mnl_vdb .task}

The first step when you start using IoT Platform is to create products. A product is a collection of devices that typically have the same features. For example, a product can refer to a product model and a device is a specific device of the product model.

IoT Platform supports two editions of products: Basic Edition and Pro Edition. This article introduces how to create a Basic Edition product.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.   In the left-side navigation pane, click Products, and then click **Create Product**. 
3.   In the dialog box that appears, select **Basic Edition**, enter a valid product name, choose a node type, and then click **OK**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12825/15404598796711_en-US.png)

    Descriptions:

    -   Product Name: Enter a name for your product. The product name will act as a unique identifier. For example, you can enter the product model as the product name.
    -   Node Type: Select either a device or gateway.
        -   Device: This type of device can be connected directly to the IoT Hub, or attached as a sub-device to a gateway.
        -   Gateway: A device that connects directly to the IoT Hub and can attach sub-devices. A gateway can manage sub-devices, maintain the topological relationship with sub-devices, and synchronize the topological relationship to the IoT Hub.

After the product is created successfully, you are automatically redirected to the Products page. You can then view or edit the product information.

