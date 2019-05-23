# Manage files {#task_j31_lmt_sfb .task}

IoT Platform allows devices to upload files over HTTP/2 channels to the Alibaba Cloud IoT Platform server for storage. After a file is uploaded, you can download and delete the file in the IoT Platform console.

-   The device is connected to IoT Platform. For more information about device SDK development, see [Link Kit SDK documentation](https://www.alibabacloud.com/help/product/93051.htm).
-   The HTTP/2 file upload function is compiled and configured on the device.

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).
2.  In the left-side navigation pane, choose **Devices** \> **Device**, and then click **View** next to the corresponding device. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/61448/155860381731465_en-US.png)

3.  On the Device Details page, click the **Manage Files** tab. 

    On the Manage Files tab page, you can view the files that were uploaded by the device through the HTTP/2 channel.

    **Note:** The maximum file size that can be stored on the IoT Platform server for each Alibaba Cloud account is 1 GB. The maximum number of files that can be stored for each device is 1,000.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/61448/155860381731618_en-US.png)

    You can perform the following operations on an uploaded file:

    |Operation|Description|
    |:--------|:----------|
    |Download|Download the file to your local device.|
    |Delete|Delete the file.|

    In addition to file management in the console, you can also query or delete files by calling the following cloud API operations: [QueryDeviceFileList](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceFileList.md#), [QueryDeviceFile](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/QueryDeviceFile.md#), and [DeleteDeviceFile](../../../../intl.en-US/Developer Guide (Cloud)/API reference/Manage devices/DeleteDeviceFile.md#).


