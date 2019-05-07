# Limits {#concept_gyg_xrt_tdb .concept}

IoT Platform has the following restrictions.

## Products and devices {#section_srj_k1v_4fb .section}

|Item|Description|Limit|
|:---|:----------|:----|
|Number of tags|The maximum number of tags that a product, device, or device group can support.|100|
|Number of products|The maximum number of products that an account can support.|1,000|
|Number of devices|The maximum number of devices that a product can support. **Note:** If your business requires more devices, [open a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) in the console and describe your requirements in the ticket.

 |500,000|
|The maximum number of devices that an account can support. **Note:** If your business requires more devices, [open a ticket](https://selfservice.console.aliyun.com/ticket/createIndex) in the console and describe your requirements in the ticket.

 |10,000,000|
|Gateways and sub-devices|The maximum number of sub-devices that a gateway can support.|1,500|
|Features for TSL modes|The maximum number of features that a product can support.|200|
|The maximum number of parameters that can be defined for a struct property.|10|
| Depends on the data type:

 -   enum: The number of enumeration items cannot exceed 25.
-   text: The data cannot contain more than 2,048 bytes.
-   array: The number of elements in an array cannot exceed 128.

 |-|
|The maximum number of output or input parameters that can be specified for a service.|20|
|The maximum number of output parameters that can be specified for an event.|50|
|Device groups|The maximum number of groups \(including parent groups and subgroups\) that an account can support.|1,000|
|The maximum number of devices that can be added to a group.|20,000|
|The maximum number of groups to which a device can be added.|10|
|Data parsing|The maximum size of the script file that can be uploaded for data parsing.|48 KB|
|Remote configuration|The remote configuration file supports only the JSON format. The file size cannot exceed 64 KB.|64 KB|
|Data storage|The data of properties, events, and services is stored for 30 days.|30 days|

## Communication {#section_x5w_ycv_4fb .section}

|Description|Limit|
|:----------|:----|
|The maximum number of MQTT requests that an account can send per second.|500|
|The maximum number of connections per minute for a device.|5|
|The maximum number of topics to which a device can subscribe. After the maximum number is reached, new subscription requests will be rejected. The device can obtain the request result by verifying the received SUBACK message.

 |100|
|The maximum number of requests sent by devices to IoT Platform per second for an account.|10,000|
|The maximum number of requests sent by IoT Platform to devices per second for an account.|2,000|
|The maximum number of messages that can be sent to the data forwarding module of the rules engine for an account.|1,000|
|The maximum number of messages to which you can subscribe per second for an account.|1,000|
|The maximum frequency in which a device can send messages to IoT Platform.|QoS0: 30 messages per second. QoS1: 10 messages per second.

 |
|A device can receive up to 50 messages per second. The performance is also subject to network conditions. If the maximum TCP write buffer size is exceeded, an error will be returned. For example, when you use the Pub operation to send requests to a device, a throttling error will be returned if the device cannot handle all the requests.

 **Note:** If you use the MQTT protocol to connect your devices to IoT Platform, no throttling error messages will be returned. However, you can view logs to detect which devices are being throttled.

 |50 messages per second|
|The maximum throughput per connection \(bandwidth\).|1,024 KB|
|IoT Platform limits the number of unacknowledged message publishing requests for a single device. After the maximum number is reached, IoT Platform will not receive any new message publishing requests for the device unless a PUBACK message is returned.

 |100|
|The maximum storage period for a message that is sent with the sending method of QoS 1. If no PUBACK message is received before the maximum storage period expires, the request will be discarded.

 |7 days|
|The maximum size of a message that is sent over MQTT. Messages that exceed this limit will be rejected.|256 KB|
|The maximum size of a message that is sent over CoAP. Messages that exceed this limit will be rejected.|1 KB|
|The heartbeat interval of an MQTT connection can be from 30 to 1,200 seconds. If the heartbeat interval is out of this range, IoT Platform will reject the connection request. We recommend that you set a value greater than 300 seconds.

 A timer starts when IoT Platform sends a CONNACK message to respond to a CONNECT message. When a PUBLISH, SUBSCRIBE, PING, or PUBACK message is received, the timer is reset. The timer is the amount of time that is equal to 1.5 times the specified heartbeat interval. If no message is received before the timer expires, the connection will be terminated.

 |30 seconds to 1,200 seconds|

## Topics {#section_mmp_bdv_4fb .section}

|Description|Limit|
|:----------|:----|
|You can define a maximum of 50 topic categories for a product.|50|
|A device can publish messages and subscribe only to its own topic.|N/A|
|A topic uses UTF-8 encoded characters and cannot contain more than 128 characters.|128 characters|
|The maximum number of slashes \(/\) that can be included in a topic.|7|
|The maximum number of topics that can be included in a subscription request.|8|
|After you subscribe to or unsubscribe from a topic, the change will take effect in approximately 10 seconds. A subscription remains effective until you unsubscribe from the topic. We recommend that you subscribe to topics in advance to avoid data loss. For example, a device sends a Sub request to topic A. After 10 seconds, the subscription takes effect and the device starts to receive messages from topic A in real time. The device will keep receiving messages from topic A unless you unsubscribe from the topic.

 |10 seconds|
|A broadcast topic allows a maximum of 1,000 devices to subscribe to it. A server SDK can send only one broadcast message to the topic per second.|1,000|

## Device shadow {#section_cnm_ddv_4fb .section}

|Description|Limit|
|:----------|:----|
|The maximum depth level of a device shadow JSON file.|5|
|The maximum size of a device shadow JSON file.|16 KB|
|The maximum number of properties in a shadow JSON file for a device.|128|
|The maximum number of requests that a device can send per second.|20|

## Data Forwarding {#section_llt_ddv_4fb .section}

|Description|Limit|
|:----------|:----|
|Each account can have up to 1,000 rules.|1,000|
|Each rule can include a maximum of 10 data forwarding actions \(10 data forwarding destinations\).|10|
|Data forwarding performance depends on the instance of the target cloud service. If the target instance has enough performance resources, Rules engine can provide a data forwarding capability of 1,000 QPS for a single account. RAM users share the quota of the corresponding Alibaba Cloud account. Data forwarding will be throttled if the number of requests exceeds the maximum number or if the target instance takes more than one second to write the received data.|1,000 QPS|
|Make sure that the instance of the target cloud service is operating correctly. Data forwarding fails if any of the following conditions exists: the crash of the target instance, overdue payments, invalid parameters such as invalid values and changed authorizations, and incorrect configurations.|-|
|Rules engine may forward messages more than one time. In a distributed environment, message balancing in real time may cause a message to be sent more than one time. The receiver must have the mechanism to ignore duplicate messages.|N/A|

## Cloud API restrictions {#section_zds_vcv_4fb .section}

|Operation name|QPS per account|QPS per IP address|
|:-------------|:--------------|:-----------------|
|Pub|1,600|100|
|RRpc|1,000|100|
|PubBroadcast|1|100|
|Other operations|50|100|


**Note:** 

-   QPS per account refers to the number of calls per second for a single Alibaba Cloud account. RAM users share the quota of the corresponding Alibaba Cloud account .
-   QPS per IP address refers to the number of calls per second for a single server that is identified by its IP address.

