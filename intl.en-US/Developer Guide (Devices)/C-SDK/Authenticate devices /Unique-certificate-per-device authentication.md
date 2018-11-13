# Unique-certificate-per-device authentication {#concept_nj5_4wg_12b .concept}

This topic describes unique-certificate-per-device authentication and how to use this authentication method.

## What is unique-certificate-per-device authentication {#section_ffs_53n_vdb .section}

IoT Platform uses unique-certificate-per-device authentication by default. The device needs to be installed with a unique device certificate in advance. When connecting to IoT Platform, the device must use ProductKey, DeviceName, and DeviceSecret for authentication. When the device passes the authentication, IoT Platform activates the device. The device and IoT Platform then can transmit data.

**Note:** Unique-certificate-per-device authentication is a secure authentication method. We recommend that you use this authentication method.

## Workflow of unique-certificate-per-device authentication {#section_irk_t1h_12b .section}

![](images/6636_en-US.png "Unique-certificate-per-device authentication")

You can use the following process to authenticate a device with unique-certificate-per-device authentication:

1.  Create a product and a device, as shown in "Create products and devices." The device obtains authentication information including ProductKey, DeviceName, and DeviceSecret.
2.  Install the ProductKey, DeviceName, and DeviceSecret on the device.
3.  Power on and connect the device to IoT Platform. The device will initiate an authentication request to IoT Platform.
4.  IoT Platform authenticates the device. If the device passes authentication, IoT Platform establishes a connection to the device, and the device begins to publish or subscribe to topics for data upload and download.

## Procedure {#section_yrd_kxg_12b .section}

1.  Use an Alibaba Cloud account to log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  Create a product and add a device to the product, as shown in [EN-US\_TP\_12819.md\#](reseller.en-US/User Guide/Create products and devices.md#).
3.  From the left-side navigation pane, select Devices. Select the device, and click **View** to obtain the ProductKey, DeviceName, and DeviceSecret, as shown in the following figure:

    ![](images/6637_en-US.png "ProductKey, DeviceName, and DeviceSecret")

4.  Download the device SDK, select a connection protocol, and configure the device SDK.
5.  Add the ProductKey, DeviceName, and DeviceSecret to the device SDK.
6.  Perform the following configurations as needed: OTA settings, sub-device connection, device TSL model development, and device shadow management.
7.  Install the device SDK that already includes the ProductKey, DeviceName, and DeviceSecret on the device.

