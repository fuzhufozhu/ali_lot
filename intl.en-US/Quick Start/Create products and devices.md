# Create products and devices {#task_p4s_ktf_vdb .task}

The first step in using IoT Platform is to create products and devices. A product is a collection of devices that typically have the same features. You can manage devices in batch by managing the corresponding product.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  Create a product. 
    1.  In the left-side navigation pane, click **Devices** \> **Product**. On the Products page, click **Create Product**. 
    2.  Enter all the required information and then click **OK**. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/15550359791955_en-US.png)

        The parameters are described as follows:

        |Parameter|Description|
        |:--------|:----------|
        |Product Name|In this example, the product is named as **TestBulb**. The product name must be unique within the account.A Product name is 4 to 30 characters in length, and can contain Chinese characters, English letters, digits and underscores. A Chinese character counts as two characters.

|
        |Category|In this example, the product category is **Custom category** indicating that features of the product is self-defined.|
        |Node Type|In this example, the node type is **Device**.        -   **Device**: Indicates that devices of this product cannot be mounted with sub-devices. This kind of devices can connect to IoT Platform directly or as sub-devices of gateway devices.
        -   **Gateway**: Indicates that devices of this product connect to IoT Platform directly and can be mounted with sub-devices. A gateway can manage sub-devices, maintain topological relationships with sub-devices, and synchronize topological relationships to IoT Platform.
|
        |Connect to Gateway**Note:** This parameter appears if the node type is **Device**.

|Indicates whether or not devices of this product can be connected to gateways as sub-devices.        -   **Yes**: Devices of this product can be connected to a gateway.
        -   **No**: Devices of this product cannot be connected to a gateway.
|
        |Network Connection Method**Note:** This parameter appears if you select **No** for **Connect to Gateway**.

|Select a network connection method for the devices. In this example, **WiFi** is selected.|
        |Data Type|Select a format in which devices exchange data with IoT Platform. In this example, **ICA Standard Data Format \(Alink JSON\)** is selected.ICA Standard Data Format \(Alink JSON\): The standard data format defined by IoT Platform for device and IoT Platform communication.

|
        |Product Description|Describe the product information. You can enter up to 100 characters.|

        Once the product is created successfully, it appears in the product list.

3.  Define features for the product. 
    1.  In the product list, find the product and click **View**. 
    2.  On the product details page, click **Define Feature**. 
    3.  Click **Add Feature** corresponding to **Self-Defined Feature**. 
    4.  Define a property. In this example, a light switch property is defined. 0 indicates turning the light on and 1 indicates turning the light off. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503597933073_en-US.png)

    5.  Define a service. For example, you can add an input parameter for adjusting the brightness of the bulb, and add an output parameter for the bulb to report the brightness contrast between the bulb and the room environment. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503597933075_en-US.png)

        The following figure shows an example of input parameter.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033078_en-US.png)

        The following figure shows an example of output parameter.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033079_en-US.png)

    6.  Define an event. You can define an event for devices to report errors. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033077_en-US.png)

        The following figure shows an example of output parameter.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033080_en-US.png)

4.  Create a device. 
    1.  In the left-side navigation pane, click **Devices** \> **Device**. 
    2.  On the device management page, click **Add Device**. Select a product to which the device to be created belongs, and then enter a name for the device \(DeviceName\). Click **OK**. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033082_en-US.png) 
    3.  Save the device certificate information. The certificate information includes ProductKey, DeviceName, and DeviceSecret. Keep this information confidential, because it is the certificate that will be used for device authentication when the device is connecting to IoT Platform. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/155503598033085_en-US.png) 

