# Push firmware to devices {#task_prw_fzz_xdb .task}

IoT Platform provides the firmware update feature. To update firmware, you must configure your devices to support Over-The-Air \(OTA\) updates. Then, in the IoT Platform console, you can upload firmware files and push update notifications to devices. This topic describes how to configure firmware updates and manage firmware versions.

Before you use the firmware update feature, make sure that you have developed your device to support OTA updates.

-   If you use device SDKs provided by Alibaba Cloud, see [OTA updates](../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Firmware update/OTA updates.md#).
-   If you use AliOS Things, see [OTA Tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

To support firmware updates, follow these rules:

-   You can have only 500 firmware files or fewer under an Alibaba cloud account.
-   The size of a firmware file is 1,000 MB or less, and the file can only be a .bin, .tar, .gz, .tar.gz, .zip, or .gzip file.
-   Limits of update batches are described as follows:

    Update batches: IoT Platform displays different batch update tasks that have been created.

    -   You can use a firmware version to create multiple update batches at the same time.
    -   One device can stay in only one ongoing update batch. \(The device is in the pending or updating state.\)
    -   You can use a firmware version to create only one dynamic update batch for a version to be updated.
    -   You can use different firmware versions to create multiple dynamic update batches for a version to be updated. However, if a device is included in multiple dynamic update policies at the same time, the system only implements the latest update policy.
-   Devices receive firmware update notifications. The corresponding limits are described as follows:
    -   Devices connected to IoT platform over MQTT can receive update notifications immediately when they are online. These devices cannot receive update notifications when they are offline. However, when they go online next time, they will receive the notifications that they have missed.
    -   Devices that use other connection protocols such as CoAP or HTTPS can receive update notifications immediately when they are online. These devices cannot receive notifications when they are offline. The system will not push the missed notifications again when they go online next time.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, choose **Maintenance** \> **Firmware Update**. 

    **Note:** To provide better services, IoT Platform allows you to manage firmware versions by product. When you use the new version of the firmware update feature for the first time, you must manually associate the previously uploaded firmware files with products. You can only associate a firmware file with one product. After you associate the existing firmware files with products, you can add new firmware files.

3.  If your devices use chip modules embedded with AliOS Things, you can activate the Secure Update feature. This is an optional feature. 

    This feature is used to ensure integrity and confidentiality of firmware. We recommend that you activate this feature. If you use Secure Update feature, the devices must validate the firmware and the corresponding firmware signature when the devices receive update notifications. For more information, see [OTA Tutorial for AliOS Things](https://github.com/alibaba/AliOS-Things/wiki/OTA-Tutorial).

    1.  Go to the Firmware Update page, and click **Secure Update**.
    2.  In the dialog box that appears, switch the Secure Update state to **Activated** for the product pending for update. When Secure Update is in the **Activated** state, click **Copy** in the Public Key column to copy the corresponding public key. You can use the public key to generate the signature for the device.
4.  On the Firmware Update page, click **New Firmware**.
5.  In the Add Firmware dialog box that appears, enter the firmware information, and upload the firmware file. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7553/15687935803946_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Type|     -   Full: When you select Full, you must upload a complete firmware file. The system pushes this complete firmware file to devices for update.
    -   Differential: When you select Differential, you must create a differential update package and upload this package to IoT Platform. Your package contains only the differences between the target version and the current existing version, so the system pushes only the differences to the device for update. \[DO NOT TRANSLATE\]

Differential updates effectively optimize the usage of device resources and minimize the consumption of traffic for pushing firmware files.

If your device uses a chip module embedded with AliOS Things, for information about how to generate a differential update package, see [OTA Diff Tools User Guide](https://github.com/alibaba/AliOS-Things/wiki/OTA-Diff-Tools--User-Guide).

 |
    |Firmware Name|Specify a firmware name. The name must be 4 to 32 characters in length, and can contain English letters, digits, and underscores \(\_\). The name cannot start with an underscore \(\_\).|
    |Firmware Version|Specify a version for this firmware. The version must be 1 to 64 characters in length, and can contain English letters, digits, periods \(.\), hyphens \(-\), and underscores \(\_\). You must set this parameter when you set Type to **Full**.

 |
    |Current Version|Specify one or more firmware versions to be updated. The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions to be updated from the drop-down list. You must set this parameter when you set Type to **Differential**.

 |
    |New Version|Specify one or more target firmware versions. You must set this parameter when you set Type to **Differential**.

 |
    |Affiliated Products|Specify the product that the specified firmware belongs to.|
    |Signature Algorithm|Only supports MD5 and SHA256.|
    |Upload Firmware|Uploads a firmware file. The size of the firmware file is 1,000 MB or less, and the file can only be a .bin, .tar, .gz, .tar.gz, .zip, or .gzip file.|

6.  In the Firmware List section, click **Validate Firmware** in the Actions column next to the firmware, and then test the firmware on one or more devices. 

    **Note:** After you upload the firmware file to IoT Platform, you must test the firmware on one or a few devices to make sure that these devices can be updated based on the firmware file. Then, you can use the firmware file to create batch update tasks and update more devices.

    |Parameter|Description|
    |:--------|:----------|
    |Current Version|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions to be updated from the drop-down list. After you specify the current versions, the devices that use these firmware versions are displayed in the **Devices to Be Validated** field.

 |
    |Devices to Be Validated|Specify one or more devices to be validated.|
    |Timeout \(Minutes\)|Specify a period. If the specified device has not been updated within this period, the update will time out. The period starts from the time when the specified device reports the update progress for the first time. Value range: 1 to 1440. Unit: minutes.|

7.  After the firmware is validated, click **Batch Update** in the Actions column next to the firmware, and in the dialog box that appears, set the corresponding parameters. Therefore, the system will push the update notifications to the specified devices. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7553/156879358010902_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Current Version|The drop-down list shows the firmware versions of all devices under the specified product. You can select one or more firmware versions to be updated from the drop-down list. You must set this parameter when you set Type to Full.

 |
    |Update Policy|     -   Static Update: only updates the activated devices that meet the specified criteria.
    -   Dynamic Update: maintains the scope of devices to be updated, including the devices that have reported the current versions and the newly activated devices.

**Note:** A firmware version supports only one dynamic update batch. If a dynamic update batch exists under the firmware, you must cancel this dynamic update batch before you create another dynamic update batch.

 |
    |Apply Update to|     -   All Devices: updates all devices under the specified product.
    -   Selected Devices: You must specify the range of devices to be updated if you select Selected Devices. Afterward, in the dialog box that appears, select the devices to be updated. The system will update the specified devices only.

**Note:** You can select multiple current versions. By default, this field shows the current versions that you have specified in the Current Version field.

    -   Phased Update: updates the specified range of devices only. This option appears when you select Static Update from the Update Policy drop-down list.

Afterward, you must specify the range of devices in the Range \(%\) field that appears. IoT Platform calculates the number of devices to be updated based on the specified range. The calculation result is rounded down. You must specify at least one device for phased update.

 |
    |Update Time|Specify the time when the device firmware is updated.     -   Update: updates the firmware immediately.
    -   Scheduled Update: updates the firmware at specified time. The specified time ranges from 5 minutes to 7 days.

**Note:** This update is applicable only when you select Static Update from the Update Policy drop-down list.

 |
    |Recipients per Minute|Specify the number of devices to which you want to push the download URLs for the firmware files per minute. Value range: 10 to 1000.|
    |Retry Interval|Specify the time for update retry after the specified update fails. Valid values:     -   Do Not Retry
    -   Retry Immediately
    -   Retry in 10 Minutes
    -   Retry in 30 Minutes
    -   Retry in 1 Hour
    -   Retry in 24 Hours
 |
    |Maximum Retries|Specify the maximum number of retries allowed after the specified update fails. Valid values:     -   1
    -   2
    -   5
 |
    |Timeout \(Minutes\)|Specify a period. If the specified device has not been updated within this period, the update will time out. The period starts from the time when the specified device reports the update progress for the first time. Value range: 1 to 1440. Unit: minutes.|


After you specify the batch update, click **View** in the Actions column next to the firmware, and on the Firmware Details page, click the Batch Management tab to check the update states.

-   Pending: the specified devices are pending for update. Two types of pending states are available: Pending \(Device offline\) and Pending \(Scheduled time: xxxx-xx-xx xx:xx:xx\)
-   Updating: the devices that have received the update notifications and have reported their update progresses to the console.
-   Update Successful: the devices that have been updated.
-   Update Failed: the devices that have failed the update. The failure causes are displayed.

    Some updates failed possibly because:

    -   The device had another update task in progress. After the device has finished the existing update task, you can try to update the device for the specified version again.
    -   During the updating process, a firmware package download failure, a firmware file extraction failure, a validation failure, or other failures occurred. In these cases, you can try updating again.

