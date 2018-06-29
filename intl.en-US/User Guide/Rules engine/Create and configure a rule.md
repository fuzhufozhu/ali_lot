# Create and configure a rule {#task_tmt_m5m_vdb .task}

This topic describes how to create and configure a rule.

1.   In the left-side navigation pane, click Rules Engine. On the displayed page, click **Create Rule**. 
2.   In the displayed dialog box, specify a rule name and select a data type. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/2331_en-US.png) 

    -   Rule Name: Enter a unique rule name. Rule names are used to identify each rule.
    -   Data Type: JSON and binary formats are supported. The rules engine processes data based on topics. Therefore, you need to select the format of the data in the topic that you want to process.
3.   Locate the rule you have created and click **Manage**. On the displayed Rule Details page, configure the rule. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/2334_en-US.png) 
    1.   Click **Edit SQL** to write a SQL statement as the rule for data processing. 

        **Note:** If the payload of your reported message is composed of binary data, fields in the payload cannot be parsed. The payload can only be filtered based on specified conditions.

        For example, the following SQL statement can be used to extract the deviceName field from the topic category that ends with the level labeled data.

         ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/2344_en-US.png) 

        For more information about how to write SQL statements, see [SQL statements](intl.en-US/User Guide/Rules engine/SQL statements.md#) and [Functions](intl.en-US/User Guide/Rules engine/Functions.md#).

    2.   Click **Add Operation** next to **Data Forwarding**. Then in the displayed dialog box, select the Alibaba Cloud service to which you want to forward the processed data, and follow the instructions on the page to configure the parameters. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/2352_en-US.png) 

        For more information about data forwarding examples, see [Examples](intl.en-US/User Guide/Rules engine/Examples.md#).

4.   Return to the Rules Engine page. Click **Enable**. Then data will be forwarded following this rule. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/2361_en-US.png) You can also perform the following operations:
    -   Click **Manage** to modify settings of this rule.
    -   Click **Delete** to delete this rule. Rules that have been enabled cannot be deleted.
    -   Click **Disable** to disable this rule.

