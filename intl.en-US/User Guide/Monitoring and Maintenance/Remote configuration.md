# Remote configuration {#concept_om1_tgs_c2b .concept}

IoT Platform provides the remote configuration function, which allows device configurations to update online when the device is in service.

## Prerequisites {#section_xlf_qp5_c2b .section}

-   You have activated the remote configuration function in the IoT Platform console. If you have not activated this function, log on to the IoT Platform console and then, in the left-side navigation pane, click **Maintenance** \> **Remote Config.**. Then, click **Enable Service**.
-   You have configured your device SDK to support the remote configuration function. Define `FEATURE_SERVICE_OTA_ENABLED = y` in the device SDK. The SDK provides the linkkit\_cota\_init operation to initialize remote configurations such as Config Over The Air \(COTA\).

## Introduction to the remote configuration function {#section_qlv_fls_c2b .section}

Developers often need to update device configurations, such as the system parameters, network parameters, and security policies of devices. Generally, device configurations are updated using the firmware update function. However, firmware update requires more time for firmware version maintenance, and devices must stop their services in order to install the update. To streamline the device configuration update process, IoT Platform provides the remote configuration function. This function enables you to complete configuration updates without service interruption.

With the remote configuration function, you can perform the following operations:

-   Enable or disable remote configuration.
-   Edit configuration files and perform version management in the IoT Platform console.
-   Update the configuration information for all devices of a product at one time.
-   Enable devices to send requests for configuration update from IoT Platform.

Remote configuration flow chart:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906235_en-US.png)

The processes involved in remote configuration include the ability to:

-   Edit and save configuration files in the IoT Platform console.
-   Push configuration updates to all devices of a product in the IoT Platform console. Then, when the devices receive the update requests, they immediately update their configurations.
-   Devices can also send requests for configuration updates from IoT Platform, and then perform update when configuration information is received.

## Use the remote configuration function {#section_dtz_hls_c2b .section}

The remote configuration function is mainly designed for two scenarios, namely, you want to push configuration updates to devices from IoT Platform, or you want to allow devices to send requests for configuration updates. The process of using the remote configuration function varies based on different scenarios.

**Scenario 1: Push configuration information to devices from IoT Platform.**

In the IoT Platform console, you can push device configuration updates to all devices of a product.

1.  Connect the devices to IoT Platform and configure the devices to subscribe to the topic `/sys/${productKey}/${deviceName}/thing/config/push`.
2.  In the IoT Platform console, edit a configuration file.
    1.  In the left-side navigation pane, click **Maintenance** \> **Remote Config.**.
    2.  Select the product for which you want to use the remote configuration function, and enable the function.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906236_en-US.png)

        **Note:** 

        -   Only if you enable the remote configuration function for the selected product can you edit a configuration template file for it.
        -   If the remote configuration function is not enabled, devices of the product cannot be updated in this way.
        -   A configuration template file that you edit here is used by all the devices of the product. Currently, you cannot push a configuration file to a specified device.
    3.  Click **Edit**, and then edit a configuration template in the area of **Configuration Template**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906237_en-US.png)

        **Note:** 

        -   Remote configuration files are JSON files. IoT Platform does not have special requirements for the configuration content. The system only checks the format of the data when you submit the configuration file. This is to prevent errors that are caused by format errors.
        -   The configuration file can be up to 64 KB. The file size is dynamically displayed in the upper-right corner of the editing area. Configuration files larger than 64 KB cannot be submitted.
    4.  After you have completed editing the configuration information, click **Save** to generate the configuration file. The system then allows devices to send requests for the configuration file.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906238_en-US.png)

3.  Push the configuration file to devices. Click **Batch Update** and then IoT Platform sends the configuration file to all the devices of the product.

    After you click **Batch Update**, the system may initiate SMS authentication to verify your account. If authentication is required, you need to first complete account verification, and then the system sends the configuration file to the devices.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906239_en-US.png)

    **Note:** 

    -   Operation frequency limit: You can only perform a batch update once per hour.
    -   If you want to stop pushing configuration updates, disable the remote configuration function for the product. The system then stops pushing the update file and will deny update requests from devices.
4.  Devices automatically update the configuration after receiving the configuration file from IoT Platform.

Configuration file management:

The latest five configuration files are saved in the console by default. After you edit and save a new version of configuration file, the previous version is automatically displayed in the configuration version record list. You can view the update time and content of the displayed five versions.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906240_en-US.png)

Click **View** to view the configuration content of the version. Click **Recover to This Version**, and the configuration content of this version will be displayed in the editing box. You can edit the content and then save it as a new version.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15552922906241_en-US.png)

**Scenario two: Devices send requests for configuration information.**

If devices are configured to send requests for configuration information, you need to enable the remote configuration function. To do so, follow these steps:

1.  Configure the devices to subscribe to the topic `/sys/${productKey}/${deviceName}/thing/config/get_reply`.
2.  In the IoT Platform console, enable the remote configuration function and edit a configuration file. For detailed steps, see the related procedures in [Scenario 1](#).
3.  Configure the devices to call the linkkit\_invoke\_cota\_get\_config operation to trigger requests for remote configuration.
4.  Configure the devices to send requests for the latest configuration updates through the topic `/sys/${productKey}/${deviceName}/thing/config/get`.
5.  IoT Platform returns the latest configuration information to the devices after receiving the requests.
6.  The devices use the cota\_callback function to process the configuration file that is sent through the remote configuration function.

