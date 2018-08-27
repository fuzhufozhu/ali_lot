# Remote configuration {#concept_om1_tgs_c2b .concept}

## Prerequisite {#section_xlf_qp5_c2b .section}

-   Make sure that you have enabled Remote Configuration. If you have not enabled this service, log on to the IoT Platform console, select Extended Services,  and click **Enable Service** under Remote Configuration.
-   By default, the device SDK has enabled Remote Configuration. You need to define `FEATURE_SERVICE_OTA_ENABLED = y` in the device SDK. The SDK provides the linkkit\_cota\_init operation to initialize remote configurations such as Config Over The Air \(COTA\).

## Introduction to Remote Configuration {#section_qlv_fls_c2b .section}

In many scenarios, developers need to update the device configuration, such as the system parameters, network parameters, and security policies of the devices. Usually, a firmware update is used to complete device configuration update. However, this approach requires more work for firmware version maintenance, and the device must stop running in order to install the update. To fix these issues, IoT Platform provides the Remote Configuration service. This service enables you to complete configuration updates without the need for device restart or service interruption.

With the Remote Configuration service, you can perform the following operations:

-   Enable or disable Remote Configuration.
-   Edit configuration files online and perform version management.
-   Update the configuration information of multiple devices.
-   Enable the device to send configuration update requests.

Remote Configuration flow chart:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516235_en-US.png)

Remote Configuration consists of the following parts:

-   A user edits and saves configuration in the IoT Platform console.
-   The user then pushes configuration updates to multiple devices that then update their local configuration file after receiving these updates.
-   The device can also send configuration update requests to IoT Platform and perform updates.

## Enable Remote Configuration {#section_dtz_hls_c2b .section}

Two scenarios are involved when you enable Remote Configuration. One scenario is that IoT Platform sends configuration updates to the device. The other scenario is that the device sends queries about configuration information.  The steps to enable Remote Configuration vary based on different scenarios.

**Scenario one**

If devices receive configuration information from the IoT platform, use the following steps to enable Remote Configuration.

1.  When the device is online, configure the device to subscribe to topic \(/sys/$\{productKey\}/$\{deviceName\}/thing/config/push\) that pushes configuration information.
2.  Enable Remote Configuration in the IoT Platform console.
    1.  Log on to the IoT Platform console, and select Extended Services.
    2.  Click **Remote Configuration** to enter the detail page, and click **Enable Service**.
    3.  Select a product and click to enable Remote Configuration.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516236_en-US.png)

        **Note:** 

        -   You must enable Remote Configuration to edit configuration information.
        -   You can also disable Remote Configuration here.
    4.  In the editing area, click **Edit** to edit configuration information. You can also copy configuration information to the editing area. The product configuration template is applicable to all devices under this product. Currently, you cannot update the configuration of individual devices.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516237_en-US.png)

        -   Remote Configuration supports JSON files. IoT Platform does not have requirements for the configuration content. The system only checks the format of the data when you submit the configuration file. This prevents configuration errors that are caused by format errors.
        -   The configuration file can be up to 64 KB in size. The file size is dynamically displayed in the upper-right corner of the editing area. Configuration files larger than 64 KB cannot be submitted.
    5.  After you have finished editing the configuration information, click **Update** to create the configuration file. This allows devices to send requests to update configuration information.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516238_en-US.png)

    6.  After the configuration file has been submitted, IoT Platform does not push updates to devices immediately. You must click **Batch Update** so that the system pushes the updated configuration file to all devices.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516239_en-US.png)

        **Note:** 

        -   You can only perform batch update once in an hour. Do not perform batch updates frequently.
        -   If you want to stop pushing configuration updates, you need to disable Remote Configuration. The system then stops pushing all updates and denies update requests from devices.
    7.  You can view configuration change history.

        Remote Configuration saves the latest five configuration changes by default. After you have submitted a configuration change, the latest configuration is displayed in the version records. You can then view the configuration information and time of update, providing high traceability of records.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826516240_en-US.png)

        Click **View** to view the configuration information of the specified version. Click **Restore to This Version** to copy the configuration information into the editing area so that you can edit and update the configuration.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14736/15353826526241_en-US.png)

3.  The device automatically updates the configuration after receiving the configuration updates from IoT Platform.

**Scenario two**

If devices need to send queries about configuration information, use the following steps to enable Remote Configuration.

1.  Configure the device to subscribe to topic \(/sys/$\{productKey\}/$\{deviceName\}/thing/config/get\_reply\).
2.  Enable Remote Configuration in the IoT Platform console. For more information, see [2](#step2).
3.  The device call the linkkit\_invoke\_cota\_get\_config operation to trigger the request for remote configuration.
4.  The device sends queries about the latest configuration updates through topic \(/sys/$\{productKey\}/$\{deviceName\}/thing/config/get\).
5.  IoT Platform returns the latest configuration information to the device after receiving the queries.
6.  The device use the cota\_callback callback to process the configuration file that is sent through Remote Configuration.

