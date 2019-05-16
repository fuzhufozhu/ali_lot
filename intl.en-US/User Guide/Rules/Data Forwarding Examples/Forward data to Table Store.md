# Forward data to Table Store {#task_ay5_3x5_vdb .task}

You can configure the rules engine data forwarding function to forward data to Table Store.

Before you configure forwarding, complete the following tasks:

-   In the IoT Platform console, create a forwarding rule and write SQL statements for data processing.

    For more information, see [Create and configure a rule](intl.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#).

    In this example, the following SQL statement is defined:

    ```
    SELECT deviceName as deviceName, items.PM25.value as PM25, items. WorkMode.value as WorkMode 
    FROM "/sys/a1ktuxe****/aircleanerthing/event/property/post" WHERE
    ```

-   In the Table Store console, create instances and tables for data receiving and storage.

    For more information about Table Store, see [Table Store documentation](https://partners-intl.aliyun.com/help/product/27278.htm) .


1.  On the Data Forwarding Rule Details page of the rule, click **Add Operation** in the **Data Forwarding** section. In the Add Operation dialog box, select **Save to Table Store**. 

    **Note:** Binary data cannot be forwarded to Table Store.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/15579747962643_en-US.png)

2.  Set parameters as prompted, and then click**OK**. 

    |Parameter|Description|
    |:--------|:----------|
    |Select Operation|Select **Save to Table Store**.|
    |Region|Select the region of the Table Store instance that receives data.|
    |Instance|Select the Table Store instance that receives data.|
    |Data Sheet|Select the table that receives data..|
    |Primary Key|To set the value for a primary key of the table, you must use the corresponding field value in the SELECT statement of the forwarding rule. When data is forwarded, this value is saved as the value of the primary key.**Note:** 

    -   You can set this parameter in the format of `${}`. For example, $\{deviceName\} indicates that the value of the primary key is the value of `DeviceName` in the message.
    -   If the primary key is an auto-increment column, you do not need to specify the value for the primary key. Table Store automatically generates a value for this primary key column. Therefore, the value of an auto-increment primary key is automatically set to `AUTO_INCREMENT` and cannot be modified.

For more information about auto-increment primary keys, see [Auto-increment function of the primary key column](https://www.alibabacloud.com/help/doc-detail/47745.htm) .

|
    |Role|Authorize IoT Platform to write data to Table Store.You must create a role with Table Store write permissions in the [RAM console](https://ram.console.aliyun.com/roles) and assign the role to IoT Platform.

|

3.  Return to the Data Forwarding Rules page, and click **Start** in the Actions column of the corresponding rule. After the rule is started, when a message is published to the topic that is defined in the SQL statement, only the message data defined by the SELECT fields is forwarded to the table in Table Store.
4.  Simulate data push to test data flow. 
    1.  In the left-side navigation pane of the IoT Platform console, choose **Maintenance** \> **Online Debug**. 
    2.  Select the device for debugging, and use a **Virtual Device** to push analog data to IoT Platform. For more information, see [Debug applications using virtual devices](intl.en-US/User Guide/Monitoring and Maintenance/Online debug/Debug applications using virtual devices.md#). 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/155797479641753_en-US.png)

    3.  After the data is pushed, go to the Data Editor page of the target table in the Table Store console to check whether the specified data has been received. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/155797479641760_en-US.png)


