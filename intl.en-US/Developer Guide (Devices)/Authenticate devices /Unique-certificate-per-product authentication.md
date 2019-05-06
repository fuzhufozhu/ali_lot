# Unique-certificate-per-product authentication {#task_m1l_zqq_wfb .task}

Using unique-certificate-per-product authentication method requires that devices of a product have been installed with a same firmware in which a product certificate \(ProductKey and ProductSecret\) has been installed. When a device initiates an activation request, IoT Platform authenticates the product certificate of the device. After the authentication is passed, IoT Platform assigns the corresponding DeviceSecret to the device.

**Note:** 

-   This authentication method has risks of product certificate leakage because all devices of a product are installed with the same firmware. On the Product Details page, you can disable Dynamic Registration to reject authentication requests from new devices.
-   The unique-certificate-per-product method is used to obtain the DeviceSecret of devices from IoT Platform. The DeviceSecret is only issued once. The device stores the DeviceSecret for future use.

Workflow of unique-certificate-per-product authentication:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14634/155710722332794_en-US.png)

1.  In the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot), create a product. For more information, see [Create a product](../../../../reseller.en-US/User Guide/Create products and devices/Create a product.md#) in the User Guide.
2.  On the Product Details page, enable Dynamic Registration. IoT Platform sends an SMS verification code to confirm your identity. 

    **Note:** If Dynamic Registration is not enabled when devices initiate activation requests, IoT Platform rejects the activation requests. Activated devices are not affected.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14634/155710722432795_en-US.png)

3.  Register a device. The status of a newly registered device is Inactive. 

    IoT Platform authenticates the DeviceName when a device initiates an activation request. We recommend that you use an identifier that can be obtained directly from the device, such as the MAC address, IMEI or serial number, as the DeviceName.

4.  Install the product certificate to the device. Follow these steps:
    1.  Download a device-side SDK.
    2.  Configure the device-side SDK to use the unique-certificate-per-product authentication method. In the device-side SDK, configure the product certificate \(ProductKey and ProductSecret\). 
    3.  Develop the device-side SDK based on your business needs, such as OTA development, sub-device connection, TSL-based device feature development, and device shadows development.
    4.  During the production process, install the developed device SDK to the device.
5.  Power on the device and connect the device to the network. The device sends an authentication request to IoT Platform to perform unique-certificate-per-product authentication.
6.  After the product certificate has been authenticated by IoT Platform, IoT Platform dynamically assigns the corresponding DeviceSecret to the device. Then, the device has obtained its device certificate \(ProductKey, DeviceName, and DeviceSecret\) and can connect to IoT Platform. After the connection with IoT Platform has been successfully established, the device can communicate with IoT Platform by publishing messages to topics and subscribing to topic messages. 

    **Note:** IoT Platform dynamically assigns DeviceSecret to devices only for the first activation of devices. If you want to reinitialize a device, go to IoT Platform console to delete the device and repeat the procedures to register and activate a device.


