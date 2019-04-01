# Forward data to another topic {#task_yvk_2c5_vdb .task}

You can forward the data that is processed based on SQL rules to another topic for machine-to-machine \(M2M\) communication and other applications.

Before configuring forwarding, follow the instructions in [Create and configure a rule](intl.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#) to write a SQL script and filter the data.

The following document describes how to forward data from Topic1 to Topic2 based on the rules engine settings:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7543/15541100162531_en-US.png)

1.   Click **Add Operation** next to **Data Forwarding**. The Add Operation page appears. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7543/15541100162628_en-US.png)

2.   Follow the instructions on the page to configure the parameters. 
    -   Select Operation: Select **Publish to Another Topic**.
    -   Topic: The topic to which the data is forwarded. You need to complete this topic after selecting a product. You can use the `${}` expression to quote the context value. For example, `${dn}/get` allows you to select the devicename from the message. The suffix of this topic is get.

