# Manage devices {#concept_h5h_q44_hhb .concept}

After you create a device in IoT Platform, you can manage or view device information in the IoT Platform console.

## Manage devices of an account {#section_d3p_lp4_hhb .section}

From the left-side navigation pane, choose **Devices** \> **Device**. The Devices page appears.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/154353/155644682043278_en-US.png)

|Task|Procedure|
|:---|:--------|
|View devices under a specific product|Select a product in the upper-left corner of the page.|
|Search for a device|Enter a device name, note name, or device tag to search for a device. Fuzzy search is supported.|
|View detailed information about a device|Click View next to the corresponding device.|
|Delete a device|Click Delete next to the corresponding device.**Note:** After a device is deleted, the device certificate becomes invalid and the data about this device in IoT Platform is deleted.

|

## View detailed information about a device {#section_ist_ht4_hhb .section}

In the device list, click **View** next to the corresponding device. The Device Details page appears.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/154353/155644682043279_en-US.png)

|Task|Procedure|
|:---|:--------|
|Activate the device|The Inactive status indicates that the device is not connected to IoT Platform. To develop the device and activate the device, see [Download device SDKs](../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).|
|View device information|View the basic information about the device, including device certificate information, [firmware information](reseller.en-US/User Guide/Monitoring and Maintenance/Firmware update.md#), [extended information](http://gitlab.alibaba-inc.com/Apsaras64/pub/wikis/Linkkit_Iterations/V230/Devinfo_Report), and tag information.|
|View device data| -   On the Status tab page, view the latest values, data records, and desired values of properties.
-   On the Events tab page, view the records about device reported events.
-   On the Invoke Service tab page, view the service call records.

 |
|View device log|On the Device Log tab page, click Read Now to view the device log information. The information include device activities, upstream messages, downstream messages, TSL data, and QoS=1 message contents. For more information about device logs, see [Device log](reseller.en-US/User Guide/Monitoring and Maintenance/Device log.md#).|

