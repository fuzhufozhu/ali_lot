# Unique-certificate-per-device authentication {#task_n21_glp_wfb .task}

Using unique-certificate-per-device authentication method requires that each device has be installed with a unique device certificate in advance. When you connect a device to IoT Platform, IoT Platform authenticates the ProductKey, DeviceName, and DeviceSecret of the device. After the authentication is passed, IoT Platform activates the device to enable data communication between the device and IoT Platform.

The unique-certificate-per-device authentication method is a secure authentication method. We recommend that you use this authentication method.

Workflow of unique-certificate-per-device authentication:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14633/155710719932767_en-US.png)

1.  In the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot), create a product. For more information, see [Create a product](../../../../reseller.en-US/User Guide/Create products and devices/Create a product.md#) in the User Guide.
2.  Register a device to the product you have created and obtain the device certificate. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14633/155710719932775_en-US.png)

3.  Install the certificate to the device. Follow these steps:
    1.  Download a device-side SDK.
    2.  Configure the device-side SDK. In the device-side SDK, configure the device certificate \(ProductKey, DeviceName, and DeviceSecret\). 
    3.  Develop the device-side SDK based on your business needs, such as OTA development, sub-device connection, TSL-based device feature development, and device shadows development.
    4.  During the production process, install the developed device SDK to the device.
4.  Power on and connect the device to IoT Platform. The device will initiate an authentication request to IoT Platform using the unique-certificate-per-product method.
5.  IoT Platform authenticates the device certificate. After the authentication is passed and the connection with IoT Platform has been established, the device can communicate with IoT Platform by publishing messages to topics and subscribing to topic messages.

