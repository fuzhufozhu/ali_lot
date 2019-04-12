# Debug applications using virtual devices {#task_nfz_212_qgb .task}

IoT Platform provides virtual devices to help developers debug applications.

A typical IoT development process is as follows: a device client is developed, the devices report data to IoT Platform, and the developers use the data to develop applications. However, this development process is time consuming. To resolve this issue, IoT Platform provides virtual devices that simulate the physical devices connecting to IoT Platform and reporting defined properties and events. You can then use the data reported by the virtual devices to debug your applications. After the physical devices connect to IoT Platform, the corresponding virtual devices will automatically become inactive.

Limits:

-   The minimum time interval for pushing data is 1 second.
-   The maximum number of messages that can be pushed at a specific interval is 1,000.
-   The maximum number of times you can use the **Push** method per day is 100.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot).
2.  In the left-side navigation pane, choose **Maintenance** \> **Online Debug**
3.  On the **Online Debugging** page, select the device to be debugged. 

    After you select a device, you are automatically directed to the debugging page.

4.  Choose **Virtual Device** \> **Start Virtual Device**. 

    **Note:** If the physical device is active or disabled, you cannot start the corresponding virtual device.

5.  Set the content for the simulated push. 
    -   If the device data type is Alink JSON, you can enter values of properties and events.

        For a property value, you can enter a value that complies with the data type and the value range of the property, or you can enter the function random\(\) to generate a random value.

        The following example shows the **Properties** page of a device, where the value 220 is entered for Voltage.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/122865/155503456638447_en-US.png)

    -   If the device data type is Do not parse/Custom, you can enter a Base64 string. The length of string cannot exceeds 4096 characters.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17811/155503456643394_en-US.png)

6.  Select a data push method. 
    -   **Push**: Push the data immediately.
    -   **Push Policy**:
        -   At Specific Time: Push the data at your specified time.
        -   At Specific Interval: Push the data regularly at your specified time interval in your specified time range. The unit of time interval is seconds.

After the push operation is executed, the operation log is displayed on the **Real-time Logs** tab page.

After the data is pushed, click **View Data** to view the device details page. On the Status tab page, you can view property information that has been pushed, and on the Events tab page you can view event information that has been pushed.

**Note:** If you have set a **Push Policy**, the data will be pushed according to the policy. After the data has been pushed, the operation log, property information, or event information will be displayed on the corresponding page.

