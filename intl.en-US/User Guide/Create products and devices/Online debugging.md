# Online debugging {#task_wpk_dtg_cgb .task}

After you have developed your device SDK, you can use the Online Debugging feature to debug it in the IoT Platform console.

1.  In the left-side navigation pane of the IoT Platform console, click **Devices** \> **Product**. 
2.  In the product list, find the product to which the device belongs, and click the **View** button corresponding to it. 
3.  On the product details page, click **Online Debugging**, and then select the device that you want to debug. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79816/154529973634149_en-US.png)

4.  Select **Debug Physical Device**. 
5.  Select a feature that you want to test. 

    If you select a property, you need to select an operation method from **Set** and **Get**. If you select an event, select **Get** as the operation method.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/79816/154529973634150_en-US.png)

6.  In the box, enter a parameter and value used for debugging. 

    -   Set a property: Enter a property value in the format of `{"YourPropertyIdentifier": Value}`, and then click **Dispatch Command**. You can see the operation result from the device log.
    -   Get a property: Click **Dispatch Command**, and then the latest property information reported by the device is displayed in the box.
    -   Call a service: Enter an input parameter in the format of `{"YourServiceInputParam": Value}`, and then click **Dispatch Command**. You can see the operation result from the device log.
    -   Get an event: Click **Dispatch Command**, and then the latest event information reported by the device is displayed in the box.

