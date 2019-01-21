# Online debugging {#task_wpk_dtg_cgb .task}

After you complete the device client configuration, you can use the online debugging function in the IoT Platform console to test and debug the client.

1.  Log on to the IoT Platform console and then, in the left-side navigation pane, click **Maintenance** \> **Online Debug**. 
2.  On the **Online Debugging** page, select the device to be debugged. 

    After you select a device, you are automatically directed to the debugging page.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79816/154805228634149_en-US.png)

3.  Select **Debug Physical Device**. 
4.  Select the feature that you want to test. 

    **Note:** 

    -   If you select a property, you need to select an operation method from **Set** and **Get**.
    -   If you select an event, select **Get** as the operation method.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79816/154805228634150_en-US.png)

5.  Dispatch the command. 

    -   Set a property: Enter a property in the format of `{"YourPropertyIdentifier": Value}`, and then click **Dispatch Command**. You can then see the operation result from the device log.
    -   Get a property: Click **Dispatch Command**. Then, the latest property information reported by the device is displayed in the box.
    -   Call a service: Enter an input parameter in the format of `{"YourServiceInputParam": Value}`, and then click **Dispatch Command**. You can then see the operation result from the device log.
    -   Get an event: Click **Dispatch Command**. Then, the latest event information reported by the device is displayed in the box.

