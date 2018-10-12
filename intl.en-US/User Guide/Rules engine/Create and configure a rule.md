# Create and configure a rule {#task_tmt_m5m_vdb .task}

This topic describes how to create and configure a rule.

1.   On the Rules page of the IoT Platform console, click **Create Rule**. 
2.   Specify a Rule Name and select a Data Type. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/15393306532331_en-US.png) 

    -   Rule Name: Enter a unique rule name. Rule names are used to identify rules.
    -   Data Type: JSON and binary formats are supported. The rules engine processes data based on topics. Therefore, you must select the format of the data in the topic that you want to process.
3.   Locate the rule you have created and click **Manage**. On the Rule Details page, configure the rule. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/15393306532334_en-US.png) 
    1.   Click **Write SQL**, and then write a SQL statement as the detailed rule for data processing. 

        **Note:** You can use `to_base64(*)` to convert binary data to a base64 string. Built-in functions and conditions are also supported.

        For example, the following SQL statement can be used to extract the deviceName field from the custom topic category, which ends with the level labeled data, of the product Basic\_Light\_001.

         ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/15393306532344_en-US.png)

        -   Rule Query Expression: You must define the Field,Topic, and Condition. The system will then automatically generate a complete rule query expression.
        -   Field: The message content field. For example, `deviceName() as deviceName`.
        -   Topic: Select a topic whose messages are to be processed.

            -   Custom: Indicates that it is a custom topic. After you select a product, you can enter a custom topic.
            -   sys: Indicates that it is a system-defined topic. If you select sys, you must select a product, a device, and a system-defined topic.
            In this example, the custom topic category of the product Basic\_Light\_001 is set.

        -   Condition: The condition by which the rule is triggered.
        For more information about how to write SQL statements, see [SQL statements](intl.en-US/User Guide/Rules engine/SQL statements.md#)and [Functions](intl.en-US/User Guide/Rules engine/Functions.md#).

    2.   Click **Add Operation** next to **Data Forwarding**. Select the Alibaba Cloud service to which you want to forward the processed data, and follow the instructions on the page to configure the parameters. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/15393306532352_en-US.png) 

        For more information about data forwarding examples, see [Examples](intl.en-US/User Guide/Rules engine/Examples.md#).

4.   Go back to the Rules page and click **Start**. Data will then be forwarded following this rule. ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7542/15393306532361_en-US.png) You can also perform the following operations:
    -   Click **Manage** to modify the settings of this rule.
    -   Click **Delete** to delete this rule. Rules that are in a running status cannot be deleted.
    -   Click **Stop** to disable this rule.

