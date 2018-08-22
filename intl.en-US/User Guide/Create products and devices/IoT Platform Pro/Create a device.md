# Create a device {#task_yk1_rnl_vdb .task}

A device must belong to a product. Devices can connect directly to IoT Platform, or be attached as sub-devices to a gateway that is connected to IoT Platform.

This document describes how to create a device that has the Pro Edition features.

1.   Log on to the [IoT Platform console](http://iot.console.aliyun.com/). 
2.   In the left-side navigation pane, click **Devices**.  
3.   On the Devices page, select the Pro Edition product you created and click **Add Device**. 

    **Note:** Make sure that you have selected the correct Pro Edition product when adding a device.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/15349038582540_en-US.png)

4.   In the dialog box that appears, enter a device name and click **OK**. 

    **Note:** The device name \(DeviceName\) must be an unique identifier within the product. It can be used as a device identifier to communicate with the IoT Hub. 

5.   In the Success dialog box, click **Quick Copy** to copy the  ProductKey, DeviceName, and DeviceSecret. Paste this information into a text file and save it in a secure location. This information will be used for future reference. The combination of ProductKey, DeviceName, and DeviceSecret is known as the key properties of a device:

    -   ProductKey: The GUID issued by IoT Platform for a product.
    -   DeviceName: User-defined unique identifier for a device. It is used for device authentication and communication.
    -   DeviceSecret: The private key issued by IoT Platform for device authentication and encryption. You need to use it in conjunction with DeviceName \(or DeviceId\).
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/15349038582569_en-US.jpg)


