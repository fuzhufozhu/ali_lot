# Forward data to Message Queue {#task_yvk_2c5_vdb .task}

You can configure the rules engine to forward the processed data to Message Queue \(MQ\). This enables messages to be reliably transmitted among devices, IoT Platform, MQ, and application servers.

Before configuring forwarding, follow the instructions in [Create and configure a rule](intl.en-US/User Guide/Rules engine/Create and configure a rule.md#) to write a SQL script and filter the data.

1.   Click **Add Operation** next to **Data Forwarding** to open the Add Operation page. Select **Send to Message Queue** .  

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7544/15469114182637_en-US.png)

2.   Follow the instructions on the page to configure the parameters. 

    -   Select Operation: Select MQ.
    -   Region: Select the location of the MQ topic where you want to forward data.
    -   Topic: Select the topic where you want to forward data.
    -   Authorization: Grant IoT Platform permission to write data to MQ topics.

        **Note:** If you are not using a Platinum Edition MQ instance, you cannot write data to a topic from a region that is different from the topic's region. Make sure that the rules engine and the MQ topic are located in the same region .  If you are using a Platinum Edition MQ instance, the region is not an issue.

    After the rule of forwarding data to MQ is enabled, IoT Platform can write data to the specified MQ topic. You can use MQ to receive and send messages. For more information, see [Subscribe to messages](https://help.aliyun.com/document_detail/29538.html).


