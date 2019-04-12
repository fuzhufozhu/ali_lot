# Firmware update {#task_prw_fzz_xdb .task}

IoT Platform provides the firmware update function. To update firmware, you need to configure your device to support OTA updates. Then, in the IoT Platform console, you can upload a firmware file and push the firmware update file to devices. This topic describes how to configure firmware updates and manage firmware file versions.

Before you use the firmware update function, make sure that you have developed your device to support OTA updates.

-   If you use device SDKs, see [OTA updates](../../../../../reseller.en-US/Developer Guide (Devices)/OTA updates.md#).
-   If you use AliOS Things, see [OTA tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki).

1.  Log on to the IoT Platform console.
2.  In the left-side navigation pane, click **Maintenance** \> **Firmware Update** 

    **Note:** To provide better services, IoT Platform now allows you to manage firmware versions by product. As such, when you use the new version of the firmware update function for the first time, you need to associate your previously uploaded firmware files with your products manually. You can only associate a firmware file to one product. After you associate your existing firmware files to products, you can add new firmware files.

3.  On the Firmware Update page, click **New Firmware**. 

    **Note:** Each Alibaba Cloud account can have up to 100 firmware files.

4.  In the Add Firmware dialog box, enter the firmware information and upload the firmware file. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7553/15550552033946_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Firmware Name|Enter a firmware name. The name must be 4 to 32 characters in length and can contain letters, numbers, Chinese characters, and underscores \(\_\). It cannot begin with an underscore.|
    |Firmware Version|Enter a version for the firmware. The version must be 1 to 64 characters in length and can contain letters, numbers, periods \(.\), hyphens \(-\), and underscores \(\_\).|
    |Product|Select the product to which the firmware belongs.|
    |Signature Algorithm|Supported signature algorithms are MD5 and SHA256.|
    |Upload Firmware|Upload a firmware file. Only files in BIN, TAR, GZ, and Zip format are supported. The size of a firmware file cannot exceed 10 MB.|

5.  \(Optional\) if your devices use chips with AliOS Things, you can use the secure update function. 

    We recommend that you activate the secure update function to ensure the integrity and confidentiality of the firmware. The secure update function requires device information for firmware verification and firmware signature verification. If you use AliOS Things, see [OTA tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki).

    1.  On the Firmware Update page, click **Secure Update**.
    2.  In the Secure Update dialog box, turn the button of the secure update function to **Activated** for the products whose devices use AliOS Things. When the secure update function is **Activated**, you can click the corresponding **Copy** button to copy the key for device signature use.
6.  In the firmware list, click the corresponding **Validate Firmware** button, and then verify whether the uploaded firmware file is available. 

    **Note:** After the firmware file is uploaded to IoT Platform, you need to test whether the firmware file is available on one or more devices. Only when you confirm that the test devices have been successfully updated can the firmware file be used for batch update. You can launch validations for a firmware to occur multiple times.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7553/155505520310898_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Pending Update Version|The drop-down box displays the current firmware versions of all devices of the product. Select one or more versions that you want to update to the new version. After you select the versions, the devices with these firmware versions will be displayed when you click the drop-down button of **DeviceName**.

 |
    |DeviceName|Select one or more devices to test the firmware file.|

    **Note:** 

    -   Devices receive the firmware update notifications:
        -   If the devices that connect to IoT Platform through MQTT are online, they will immediately receive the update notifications. If the devices are offline, the system will push the update notifications to the devices when they go online again.
        -   If the devices using other connection protocols \(such as CoAP or HTTPS\) are online, they will immediately receive the update notifications. If the devices are offline, they cannot receive the notifications.
    -   Provided that you perform a firmware validation operation, the firmware status will change from **Unverified** to **Verified**. However, the status of the firmware does not indicate that the test devices have been updated successfully or that the firmware file is available. Click **Update Details** to see the update result.
7.  Click **Batch Update**, configure an update method, and then push update notifications to devices. 

    **Note:** Make sure that the firmware file has successfully passed the verification before you perform a batch update.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7553/155505520310902_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Pending Update Version|The drop-down box displays the current firmware versions of all devices of the product. Select one or more versions that you want to update to the new version.|
    |Update Policy|     -   Static Update: Only update activated devices that meet the specified criteria.
    -   Dynamic Update: All devices that meets the specified criteria receive an update notification. If you select Dynamic Update, the system maintains the scope of devices that need to be updated, including devices that have reported the current versions and newly activated devices.
 |
    |Update Region|     -   All Devices: All devices that belong to the product will be updated.
    -   Directional Upgrade: If you select Directional Upgrade, Device Range field will appear. You then need to select devices to be updated. Only selected devices will be updated.

**Note:** You can select multiple pending versions if you select to update specified devices. The version that you previously selected for update is selected by default. If you have not specified any version, all versions are selected by default.

 |
    |Update Time|Specify a time when the update performs.     -   Update Now: Update immediately after the request is submitted.
    -   Scheduled Update: Manually specify a time for the system to push the update requests to devices. You can specify a time in the range of five minutes to seven days later.

**Note:** Scheduled Update is available only when the update policy is Static Update.

If you specify a scheduled update time, in the Pending tab page of Firmware Details, you can see the scheduled update time.

 |
    |Retry After Failed Update|Configure that when the system retries to send update request again if the update fails. Options:     -   Do Not Retry
    -   Retry Immediately
    -   Retry in 10 Minutes
    -   Retry in 30 Minutes
    -   Retry in 1 hour
    -   Retry in 24 hours
 |
    |Max. Retry Times|Select how many times the system can retry. Options:     -   1
    -   2
    -   5
 |


Click **Update Details** to view the update status.

-   Pending: This tab page lists the devices which are selected for update. Two types of pending status are available: Pending \(Device offline\) and Pending \(Scheduled time: xxxx-xx-xx xx:xx:xx\)
    -   If the device is offline and the update time is scheduled for a later time, the status is shown as Pending \(Scheduled time: xxxx-xx-xx xx:xx:xx\).
    -   When it reaches the scheduled time, and the device is still offline, the status will change to Pending \(Device offline\).
-   Updating: This tab page lists the devices that have received the update notifications and have reported their update progresses to the console. If no update progress is received from the device, the progress ratio is 0.
-   Update Successful: This tab page lists the devices which have been successfully updated.
-   Update Failed: This tab page lists the devices that have failed the update and provides the reasons. The following are some causes of update failures:
    -   The device has another update task in progress. After the device has finished the current update task, you can try to update it for this version again.
    -   During the updating progress, a firmware package download failure, firmware file extraction failure, verification failure, or other failures occurred. In these cases, you can try updating again.

Click **Versions** on the Firmware Update page and then select a product to view the firmware used by the devices of the product.

-   Version Distribution: Displays the percentages of firmware usages in the product. Names and versions of the top five firmware are displayed, and other firmware are grouped in Others.
-   Versions and Devices: Displays all the firmware versions used by devices of the product and the number of devices that use the versions.
-   Device List: Displays all the devices of the product. You can select a firmware version to view the devices that use this version.

