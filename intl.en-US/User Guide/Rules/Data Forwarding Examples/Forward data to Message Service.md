# Forward data to Message Service {#task_dt1_f3c_wdb .concept}

By using rules engine to forward data from IoT Platform to [Message Service \(MNS\)](https://partners-intl.aliyun.com/help/product/27412.htm). The message transmission performance between devices and servers is improved. The advantages are described in the following section.

## Data forwarding {#section_as2_sfj_wdb .section}

-   Devices send data to application servers

    Devices send messages to IoT Platform, where the messages are processed with rules engine and forwarded to specified MNS topics. The application server can then call the relevant APIs of MNS to subscribe to topics for messages from devices.

    One advantage of this method is that using MNS to receive and store messages prevents message packet loss during server downtime. Another advantage is that MNS can process a massive amount of messages simultaneously, which means services remain available even if the server has to process a number of concurrent tasks.

-   Application servers send data to devices

    The application server calls the relevant APIs of IoT Platform to publish messages to IoT Platform, and devices subscribe to related topics for messages from the server.


![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7548/15487573714797_en-US.png)

## Procedure {#section_ogz_4m1_1gb .section}

1.  Log on to the [RAM console](https://partners-intl.console.aliyun.com/#/ram), and create a role with the permission to write messages from IoT Platform into MNS.

    Then, when you are configuring the data forwarding rule in IoT Platform, you can apply this role to allow IoT Platform to write data into MNS. Without applying such a role, IoT Platform cannot forward data to MNS.

    For more information about roles, see [RAM role management](https://partners-intl.aliyun.com/help/doc-detail/93691.htm).

2.  In the [MNS console](https://partners-intl.console.aliyun.com/#/mns), create a topic that is to receive messages from IoT Platform.
    1.  Click **Topics** \> **Create Topic**.
    2.  In the Create Topic dialog box, enter a name for the topic, and then click **OK**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7548/154875737133812_en-US.png)

    3.  On the Topic List page, find the topic and click **Subscription List** in the Actions column.
    4.  On the Subscription List page, click **Subscribe**.
    5.  Create a subscriber for this topic. A subscriber is a server that subscribes to the topic for messages from IoT Platform.

        An MNS topic can have multiple subscribers.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7548/154875737133815_en-US.png)

        For more information, see the [MNS documentations](https://partners-intl.aliyun.com/help/product/27412.htm).

3.  Go to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot) and, on the Rules page, click **Create Rule**and then create a rule
4.  Go back to the Rules page, find the newly created rule and click **Manage** on the right.
5.  On the Data Flow Details page, write the SQL statement that is used to process and filter messages. For more information, see [Create and configure a rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#) and [SQL statements](reseller.en-US/User Guide/Rules/Data Forwarding/SQL statements.md#).
6.  On the Data Flow Details page, click **Add Operation** next to **Data Forwarding**.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7548/15487573713029_en-US.png)
7.  In the Add Operation dialog box, enter information of the MNS topic.

    Parameter description:

    |Parameter|Description|
    |:--------|:----------|
    |Select Operation|Select the Alibaba Cloud product which will be the data forwarding target. Here, select **Send to Message Service**.|
    |Region|Select the region where the MNS topic is.|
    |Theme|Select the MNS topic that is to receive data from IoT Platform.|
    |Role|The role with the permission that IoT Platform can write data into MNS.|

8.  On the Rules page, click **Start** corresponding to this rule to run the rule.

    Then, IoT Platform can forward messages of the specified IoT Platform topic to the specified MNS topic.


