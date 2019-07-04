# Create a device {#task_yk1_rnl_vdb .task}

A product is a collection of devices. After you have created a product, you must register devices under the product with IoT Platform. You can create devices individually or create multiple devices at one time. This topic describes how to create devices individually.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, choose **Devices** \> **Device**, and then click **Add Device**.
3.  In the Add Device dialog box, enter the device information and click **OK**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/15622051752540_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Product|Select a product. The device to be created will be assigned the features and properties of the selected product. **Note:** If the product is associated with another platform, make sure that your account has sufficient activation codes to create the device.

 |
    |DeviceName|Set the device name. If you left this parameter empty, the system automatically generates a device name that contains numbers and letters.     -   The device name is unique within the product.
    -   The device name must be 4 to 32 characters in length and can contain letters, numbers, and special characters. The supported special characters are hyphens \(-\), underscores \(\_\), at signs \(@\), periods \(.\) , and colons \(:\).
 |
    |Note name|Set the alias. The alias must be 4 to 64 characters in length and can contain Chinese characters, letters, numbers, and underscores \(\_\). One Chinese character is counted as two characters.|


After the device is created, the View Device Certificate dialog box appears automatically. You can view and copy the device certificate information. A device certificate is the authentication certificate of a device when the device is communicating with IoT Platform. It contains three key fields: ProductKey, DeviceName, and DeviceSecret.

|Parameter|Description|
|:--------|:----------|
|ProductKey|The key of the product to which the device belongs. It is the GUID that is issued by IoT Platform to the product.|
|DeviceName|The unique identifier of the device within the product. A device uses the DeviceName and the ProductKey as the device identifier to authenticate to and communicate with IoT Platform.|
|DeviceSecret|The device key issued by IoT Platform for device authentication and encryption. It must be used in pairs with the DeviceName.|

You can also click **View** next to the newly created device on the Device List page. On the Device Details page, click the Device Information tab to view device information.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12828/156220517533334_en-US.png)

Follow instructions in [Device development documentation](../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#) to develop the device SDK.

