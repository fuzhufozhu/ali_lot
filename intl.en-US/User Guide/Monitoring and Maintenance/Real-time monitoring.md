# Real-time monitoring {#concept_r3s_jff_4fb .concept}

In the IoT Platform console, the Real-time Monitoring page displays the number of online devices, the number of upstream and downstream messages, and the number of messages that were forwarded by the rules engine. In addition, you can set CloudMonitor alert rules to monitor the resource usage of your IoT Platform and receive alerts.

## Display data {#section_kzj_3t5_4fb .section}

To view real-time monitoring data, follow these steps:

1.  Log on to the [IoT Platform console](http://iot.console.aliyun.com/).
2.  In the left-side navigation pane, choose **Maintenance** \> **Real-time Monitoring**.
3.  Select the products and time range of the data to be viewed.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24121/155893036414296_en-US.png)

    |Time range|Description|
    |:---------|:----------|
    |1 Hour|Displays the statistics of the last hour. Statistics are collected every 1 minute.|
    |1 Day|Displays the statistics of the last 24 hours. Statistics are collected every 5 minutes.|
    |1 Week|Displays the statistics of the last seven days. Statistics are collected every 15 minutes.|

    **Note:** The abscissa values displayed on the Real-time Monitoring page do not represent the collection cycle.


The following table describes the statistical data on the Real-time Monitoring page.

|Data|Description|
|:---|:----------|
|Online Devices| The number of devices that have established persistent connections with IoT Platform.

 The data is collected with delays. A displayed value is the average value within a collection cycle. The data is displayed based on the protocol that is used to communicate with IoT Platform.

 |
|Messages Sent to IoT Platform| The number of messages that the devices send to IoT Platform.

 The data is collected with delays. A displayed value is the total value within a collection cycle. The data is displayed based on the protocol that is used to communicate with IoT Platform.

 |
|Messages Sent from IoT Platform| The number of messages sent from IoT Platform to devices and servers.

 The data is collected with delays. A displayed value is the total value within a collection cycle. The data is displayed based on the protocol that is used to communicate with IoT Platform.

 |
|Messages Forwarded Through Rule Engine|The number of messages forwarded by the rules engine data forwarding function. The data is collected with delays. A displayed value is the total value within a collection cycle. The data is displayed based on the target cloud service to which the messages were forwarded.

 |

## Alarm Config. {#section_xpq_vr5_4fb .section}

On the Real-time Monitoring page, click **Alarm Config.**. The Create Alarm Rule page of the CloudMonitor console appears. You can also directly access the [Create Alarm Rule](https://cloudmonitor.console.aliyun.com/#/alarmservice/product=&searchValue=&searchType=&searchProduct=) page to create an alert.

You can create threshold-triggered and event-triggered alert rules in the CloudMonitor console.

-   Create a threshold-triggered alert rule

    IoT Platform allows you to use CloudMonitor to monitor IoT Platform by multiple metrics. These metrics include the number of real-time online devices using a specific communication protocol, the number of messages sent from devices to IoT Platform, the number of messages sent from IoT Platform to devices, the number of messages forwarded by the rules engine to other Alibaba Cloud service, the number of property report failures, the number of event report failures, the number of service call failures, and the number of property setting failures.

    On the Create Alarm Rule page, configure the parameters and then click **Confirm**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24121/155893036445333_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Product|Select **IoT Platform**.|
    |Resource Range|Includes the following value options:     -   All Resources: An alert is sent when any instance under your IoT Platform service meets the description of the alert rule.
    -   Instance: An alert is sent only when the specified products meet the description of the alert rule.
 |
    |Region|This parameter is available only when you set Resource Range to **Instance**. Select the region of the IoT Platform instance monitored by this alert rule.|
    |Instance|Select an IoT Platform instance to be monitored and select one or more products.|
    |Alarm Rule|Set the name of the alert rule.|
    |Rule Description|Set the description of the alert rule. It defines the condition in which an alert is triggered. You must configure the following items:     -   Select a monitoring metric for the rule.
    -   Select a scan period for the rule. For example, if the scan period is set to 60 minutes, scans are performed every 60 minutes.
    -   Set the triggering condition. For example, an alert is triggered only if the number of devices exceeds 5,000 for three consecutive scan periods.
 |
    |Mute for|Set the period of time before which the alert is sent again if the exception persists after the alert is triggered.|
    |Effective Period|Set the time range when the alert rule is applied. CloudMonitor applies the alert rule to monitor the specified metric only during the specified effective period.|
    |Notification Method|Set notification parameters, such as the notification contacts and notification methods.|

    For more information about setting threshold-triggered alert rules, see [Procedure](../../../../intl.en-US/User Guide/Alarm service/Alarm rules/Create a threshold alert rule.md#section_wfl_pv4_mgb).

-   Create an event-triggered alert rule

    You can use an event-triggered alert rule to monitor the following IoT Platform events:

    -   The upstream QPS of any device reaches the upper limit.
    -   The downstream QPS of any device reaches the upper limit.
    -   The number of connection requests per second for the current account reaches the upper limit.
    -   The upstream QPS for the current account reaches the upper limit.
    -   The downstream QPS for the current account reaches the upper limit.
    -   The engines rule data forwarding QPS for the current account reaches the upper limit.
    Go to the [Alarm Rules](https://cloudmonitor.console.aliyun.com/#/alarmservice/product=&searchValue=&searchType=&searchProduct=) page of the CloudMonitor console, and choose **Event Alarm** \> **Create Event Alert**.

    The following figure shows how to create an event-triggered alert rule.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/24121/155893036445336_en-US.png)

    |Parameter|Description|
    |:--------|:----------|
    |Alarm Rule Name|Set the name of the alert rule.|
    |Event Type|Select **System Event**.|
    |Product Type|Select **IoT Platform**.|
    |Event Type|Select **All types** or **Exception**.|
    |Event Level|Select **All Levels** or select one or more specific event levels.|
    |Event Name|Select one or more events to be monitored.|
    |Resource Range|Select **All Resources**.|
    |Alarm Type|Set the alert contacts and notification methods.|

    For more information about setting event-triggered alert rules, see [Create an event alert rule](../../../../intl.en-US/User Guide/Alarm service/Alarm rules/Create an event alert rule.md#) in the CloudMonitor documentation.


