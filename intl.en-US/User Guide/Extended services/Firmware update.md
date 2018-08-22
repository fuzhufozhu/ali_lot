# Firmware update {#task_prw_fzz_xdb .task}

This topic describes how to use the firmware update service in the IoT Platform console. Currently, the firmware update service is only available in the China \(Shanghai\) region.

-   Make sure that you have enabled the firmware update service. If you have not enabled the service, log on to the IoT Platform console, select Extended Services, and click **Enable Service** under Firmware update.
-   By default, the device SDK has enabled the firmware update service.

The firmware update process includes the following tasks:

1.  Add a firmware.
2.  Validate a firmware.
3.  Batch update.
4.  Update another firmware.

Follow these steps to update a firmware:

1.   Log on to the IoT Platform console. 
2.   Select the China \(Shanghai\) region. 
3.   Add a firmware. 
    1.   Select **My Services** \> **Firmware update**. 
    2.   Click **New Firmware** on the Update Firmware page, as shown in figure [Figure 1](#fig_dyw_k11_ydb). 

        ![](images/3946_en-US.png "Add a firmware.")

    3.   Set firmware parameters. 
        -   The firmware file name can only contain Chinese characters, English letters, numbers, and underscores \(\_\) and must be 1 to 32 characters in length.
        -   The size of each firmware file must be no larger than 10 MB.
        -   You can upload up to 100 firmware files.

            **Note:** The 100 firmware files also include firmware files that have been uploaded and then deleted.

4.   Validate the firmware. 

    After you have added the firmware, you must test the firmware on a small number of devices to check whether the firmware runs correctly. If the firmware runs correctly, you can then push the firmware to all devices.

    Select a firmware from the firmware list, and click **Validate Firmware**.

    -   The system then sends a firmware update notification to all devices connected through Message Queuing Telemetry Transport \(MQTT\). Only online devices will receive the update notification. For offline devices, the system will resend an update notification to these devices when they come online.

        Connections established by using other protocols, such as CoAP and HTTPS, are transient connections. Devices using transient connections cannot receive the firmware update notification when they are offline.

    -   Firmware verification is to test the firmware on a number of devices. You can validate a firmware multiple times.
    -   Once you have verified a firmware, the status of the firmware is set to verified, regardless of the update results on the devices.
    -   The system records all update operations on the devices after the firmware verification process. The system determines that the update process has started only after a device has received the update notification and updated the update progress to the system. The system then changes the update status to Upgrading. After the update process begins, you can view the update progress on the firmware details page.
5.   After you have verified and confirmed that the firmware can run correctly, you can then update the firmware on all devcies. Select the firmware from the firmware list, and click **Batch Update**. 

    Batch update is to push the firmware update notification to a large number of devices.

    -   You cannot use a firmware that has not been verified to perform a batch update.
    -   An update is progressive, from the reception of the update notification to the completion of the update. The devices automatically send update information to the OTA system to update their update progress.
    -   During a batch update, a device may fail to update its firmware if it has not finished the last update task.
    -   If an update error occurs on a device during the update process, the device sends a notification to the OTA system. The system then sets the update status to Completed and determines that the device has failed to update the firmware. update errors include downloading failure, verification failure, and extraction failure.
    -   You can view information about batch upgrades on the firmware details page. The update failure list shows brief information about the cause of the update failures.
    When creating a batch update task, if you specify a firmware version that has already been specified in another batch update task for the same product, the system displays a message indicating that an update task conflict has occurred.

    For example, you have added firmware versions B and C to the IoT Platform console. You want to update a device with firmware version A for a product. You have also created a batch update task to update the firmware from version A to B. If you try to create another batch update task to update the firmware from version A to C, an update task conflict occurs in the console.

6.   If a device failed an update task, you can use the update operation records to re-initiate the update task. 

