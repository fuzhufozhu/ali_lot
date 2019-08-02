# Alerts and notifications {#concept_978441 .concept}

When the resource usage on IoT Platform reaches the value specified in an alert rule, the corresponding alert is triggered. Alibaba Cloud then sends a notification to the specified contact group. This topic describes the alerts and notifications of IoT Platform.

## Notifications for threshold alerts {#section_m1n_k5w_8wq .section}

When a threshold alert is triggered, the alert contact group receives a notification. The notification includes information shown in the following figure:

|Field|Description|
|:----|:----------|
|IoT Platform instance|The instance that triggers the alert. This field contains the product key \(ProductKey\), instance ID \(instanceId\), and region ID \(regionId\).|
|Metric|The metric is displayed as a code. It indicates the metric that you selected when you set the Rule Description parameter. In this example, the code "MessageCountForwardedThroughRuleEngine\_MNS" represents the number of messages forwarded by the rules engine. If the number of messages exceeds the specified threshold during a time period, an alert is triggered.

 For more information about metrics and descriptions, see the following table: Metric codes and descriptions.

 |
|Alert time|The time when the alert is triggered.|
|Count|The total number of messages, the number of forwarded messages, or the number of connected devices counted for the specified metric.|
|Duration|The time period during which an alert is triggered upon a threshold violation.|
|Rule details|The alert rule details that you have set in the CloudMonitor console.|

|Code|Description|
|----|:----------|
|MessageCountForwardedThroughRuleEngine\_FC|The number of messages forwarded by the rules engine to Function Compute.|
|MessageCountForwardedThroughRuleEngine\_MNS|The number of messages forwarded by the rules engine to Message Service.|
|MessageCountForwardedThroughRuleEngine\_OTS|The number of messages forwarded by the rules engine. It equals the number of times that the rules engine forwards data to Table Store.|
|MessageCountForwardedThroughRuleEngine\_RDS|The number of messages forwarded by the rules engine to ApsaraDB for RDS.|
|MessageCountForwardedThroughRuleEngine\_REPUBLISH|The number of messages forwarded by the rules engine from the current topic to other topics.|
|MessageCountSentFromIoT\_HTTP\_2|The number of messages that are sent through IoT Platform over HTTP/2.|
|MessageCountSentFromIoT\_MQTT|The number of messages that are sent through IoT Platform over MQTT.|
|MessageCountSentToIoT\_CoAP|The number of messages that are sent through IoT Platform over CoAP.|
|MessageCountSentToIoT\_HTTP|The number of messages that are sent to IoT Platform over HTTP.|
|MessageCountSentToIoT\_HTTP/2|The number of messages that are sent to IoT Platform over HTTP/2.|
|MessageCountSentToIoT\_MQTT|The number of messages that are sent to IoT Platform over MQTT.|
|OnlineDevicesCount\_MQTT|The number of devices that are connected to IoT Platform over MQTT in real time.|
|DeviceEventReportError|The number of event reporting failures.|
|DevicePropertyReportError|The number of property reporting failures.|
|DevicePropertySettingError|The number of property setting failures.|
|DeviceServiceCallError|The number of service calling failures.|

## Notifications for event alerts {#section_bzg_fhq_0w4 .section}

When an event alert is triggered, Alibaba Cloud sends a notification to the specified contact group.

|Field|Description|
|:----|:----------|
|Event name|The event name is displayed as a code. In this example, the code "Device\_Connect\_QPM\_Limit" represents the event of the maximum connection requests sent per minute by a device reaching the upper limit. For more information about event codes and descriptions, see the following table: Event codes and descriptions.

 |
|Object|The resource that triggers the alert. -   resourceId: The resource ID. Format:

    ``` {#codeblock_tey_kti_gn7}
acs:iot:$regionid::instance/$instanceId/product/$productKey/device/$deviceName
    ```

-   Resource name: The instance ID. iot-public indicates that this instance is a public instance.
-   Group ID: The ID of the group that the device belongs to. If the device does not belong to any group, the field displays an empty string.

 |
|Event level|Currently, all events are WARN events.|
|Event time|The time when the event occurs.|
|Event status|Currently, all events are set to the Fail status. This status indicates that the request failed because the number of connection requests sent per minute or messages sent per second has reached the upper limit.|
|Details|The information about the resource that triggers the alert. The information is in the JSON format. This field contains the region ID \(regionId\), instance ID \(instanceId\), product key \(ProductKey\), and the device name \(DeviceName\). The ProductKey and DeviceName parameters appear in the notification only when the number of connection requests sent per minute, messages sent per second, or messages received per second by a device reaches the upper limit.|

|Code|Description|
|:---|:----------|
|Device\_Connect\_QPM\_Limit|The number of connection requests sent per minute by a device has reached the upper limit.|
|Device\_Uplink\_QPS\_Limit|The number of messages sent per second by a device has reached the upper limit.|
|Device\_Downlink\_QPS\_Limit|The number of messages received per second by a device has reached the upper limit.|
|Account\_Connect\_QPS\_Limit|The number of connection requests sent per second by the current account has reached the upper limit.|
|Account\_Uplink\_QPS\_Limit|The number of messages sent per second by the current account has reached the upper limit.|
|Account\_Downlink\_QPS\_Limit|The number of messages received per second by the current account has reached the upper limit.|
|Account\_RuleEngine\_DataForward\_QPS\_Limit|The number of messages forwarded per second by the rules engine for the current account has reached the upper limit.|

