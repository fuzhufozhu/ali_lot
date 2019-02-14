# Create a device {#task_yk1_rnl_vdb .task}

A product is a collection of devices. After you have created products, you can create devices of the product models. You can create one device or multiple devices at a time. This article introduces how to create a single device.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  In the left-side navigation pane, click **Devices** \> **Device**, and then click **Add Device** 
3.  Select a product that you have created. The device to be created will be assigned with the features of the selected product. 
4.  \(Optional\) Enter a name for the device. If you do not enter a device name for the device, the system will automatically generate one for the device. 

    **Note:** A DeviceName \(device name\) must be unique within a product. It is used as a device identifier when the device communicates with IoT Platform.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/15501132122540_en-US.png)

5.  Click **OK** to create the device. After the device has been successfully created, the View Device Certificate box is displayed. There, you can view and copy the device certificate information. A device certificate is the authentication certificate of a device when the device is communicating with IoT Platform. It contains three key fields: ProductKey, DeviceName, and DeviceSecret.

    -   ProductKey: The globally unique identifier issued by IoT Platform for a product.
    -   DeviceName: The identifier of a device. It must be unique within a product and is used for device authentication and message communication.
    -   DeviceSecret: The secret key issued by IoT Platform for a device. It is used for authentication encryption and must be used in pairs with the DeviceName.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/15501132122569_en-US.jpg)

6.  On the device list page, find the device and click **View**. On the Device Details page, you can view the information of this device. On the Device Details page, click **Test** to text the network latency for the device.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/155011321233334_en-US.png)


