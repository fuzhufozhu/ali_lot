# Create a device {#task_xpz_nnl_vdb .task}

A device must belong to a product. Devices can connect directly to IoT Platform, or connect as sub-devices to a gateway that is connected to IoT Platform.

This document describes how to create a device that has the Basic Edition features.

1.   Log on to the [IoT Platform console](http://iot.console.aliyun.com/). 
2.   In the left-side navigation pane, click **Devices**.  
3.   On the Devices page, select the the Basic Edition product you created, and then click **Add Device**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12826/2290_en-US.png)

4.   In the dialog box that appears, enter a device name and click **OK**. 

    **Note:** The device name \(DeviceName\) must be a unique identifier within the product. It can be used as a device identifier to communicate with the IoT Hub. 

5.   In the Success dialog box, click **Quick Copy** to copy the  ProductKey, DeviceName, and DeviceSecret. Paste this information into a text file and save it in a secure location. You may need this information for future reference. The combination of ProductKey, DeviceName, and DeviceSecret is referred to as the key properties of the device:

    -   ProductKey: The globally unique identifier issued by IoT Platform for a product.
    -   DeviceName: User-defined unique identifier for a device. It is used for device authentication and communication.
    -   DeviceSecret: The private key issued by IoT Platform for device authentication and encryption. You need to use it in conjunction with DeviceName \(or DeviceId\).
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12826/2292_en-US.png)


The device is created successfully. IoT Platform will automatically create a corresponding Topic for the device based on the Topic Category in the Message Queue  of the product. The device can carry out Pub/Sub communications based on the permissions defined in the Topic.

