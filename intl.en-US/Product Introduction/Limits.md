# Limits {#concept_gyg_xrt_tdb .concept}

IoT Platform has the following limits.

## Products and devices {#section_srj_k1v_4fb .section}

|Item|Description|Limit|
|:---|:----------|:----|
|Total tags|The maximum number of tags that a product, device, or device group can have.|100|
|Total products|The maximum number of products that an account can have.|1,000|
|Total devices|The maximum number of devices that a product can have.**Note:** If your business requires more devices, open a ticket in the Alibaba Cloud console and describe your requirements in the ticket.

|500,000|
|Gateways and sub-devices|The maximum number of sub-devices that a gateway can have.|1,500|
|The maximum number of sub-device channels that a gateway can have.|1,000|
|The frequency in which sub-device configurations can be sent to the gateway.|Once every ten minutes.|
|Features \(only for Pro Edition products\)|The maximum number of features that a product can have.|200|
|The maximum number of struct properties.|10|
| If the data type of a feature is:

 -   enum, the number of enumeration items cannot exceed 25.
-   text, the data cannot exceed 1,024 bytes.
-   array, the number of elements in an array cannot exceed 128.

 |-|
|Each service can have a maximum of 20 input parameters and 20 output parameters.|20|
|Each event can have a maximum of 20 output parameters.|20|
|Data parsing|The size of a data parsing script cannot exceed 48 KB.|48 KB|
|Remote configuration|The remote configuration file can only be in JSON format and cannot exceed 64 KB.|64 KB|
|Data storage|The data of Pro edition properties, events, and services is stored for 30 days.|30 days|

## Communication {#section_x5w_ycv_4fb .section}

|Description|Limit|
|:----------|:----|
|The maximum number of MQTT requests that each account can send per second.|500|
|The maximum number of MQTT requests that each device can send per second.|1|
|The maximum number of connection times that each device can try per minute.|5|
|The maximum number of message subscriptions that each device can have. If the maximum number is reached, new subscription request will be rejected. The device client can get request results by verifying the SUBACK messages.|100|
|The maximum number of requests that devices of an account can send to IoT Platform per second.|10,000|
|The maximum number of requests that an account can send from IoT Platform to devices per second.|2,000|
|The maximum number of messages that can be sent to the Rules Engine per second for an account.|1,000|
|The maximum frequency in which a device can send messages to IoT Platform.|QoS 0: 30 messages per second.QoS 1: 10 messages per second.

|
|A device can receive messages in a frequency of 50 messages per second, but the Internet condition affects the performance. If the TCP write buffer is blocked, an error is returned. For example, if you use the Pub operation to send requests to a device, but the device cannot handle all the requests, you will receive an error due to throttling.**Note:** If you use the MQTT protocol to connect your devices and IoT Platform, you cannot receive error messages resulting from throttling. However, you can view logs to detect which devices are being throttled.

|50 messages per second|
|Bandwidth.|1,024 KB|
|The maximum number of unconfirmed message publishing requests that a single client can have. When the limitation is reached, no new publishing request from the client will be accepted, unless the server returns PUBACK messages.|100|
|The maximum storage time for messages that are sent with the sending method of QoS 1. If no PUBACK message from the client is received after the maximum storage time is used up, the request will be discarded.|7 days|
|The maximum size of a single message that is sent using MQTT. Messages exceed the size limit will be rejected.|256 KB|
|The maximum size of a single message that is sent using CoAP. Messages exceed the size limit will be rejected.|1 KB|
|The heartbeat interval of an MQTT connection can be 30 to 1200 seconds. If the heartbeat time is not within this range, the server will refuse to connect.We recommend that you set a value larger than 300 seconds.

A heartbeat interval begins at the time when IoT Platform sends a CONNACK message to respond to a CONNECT message. When a message of PUBLISH, SUBSCRIBE, PING or PUBACK is received, the timer is reset. If no message is received after an amount of time has passed that is equal to 1.5 times the specified heartbeat interval, the connection will be closed.

|30 to 1,200 seconds|

## Topics {#section_mmp_bdv_4fb .section}

|Description|Limit|
|:----------|:----|
|A product can have a maximum of 50 topic categories.|50|
|A device can only publish messages to and subscribe to its own topics.|-|
|Topics use UTF-8 encoded characters and cannot exceed 128 bytes.|128 bytes|
|The maximum number of slashes \(/\) can be included in a single topic.|7|
|The maximum number of messages to which servers of an account can subscribe per second.|1,000|
|The maximum number of topics that can be included in a subscription request.|8|
|After you subscribe to or unsubscribe from a topic, the change will take effect in 10 seconds. Subscriptions remain in effect until you unsubscribe from the topic. We recommend that you subscribe to topics in advance to avoid missing information.Example: A device sends a SUB request to topic A. After 10 seconds, the subscription takes effect and the device starts to receive messages in real time. The device will keep receiving messages from topic A.

|10 seconds|
|Broadcast topic. A maximum of 1,000 devices can subscribe to the same broadcast topic. The server SDK can only send one broadcast per second.|1,000|

## Device shadow {#section_cnm_ddv_4fb .section}

|Description|Limit|
|:----------|:----|
|The maximum depth level of a device shadow JSON file.|5|
|The maximum size of a device shadow JSON file.|16 KB|
|The maximum number of attributes in a device shadow JSON file.|128|
|The maximum number of requests that a device can send per second.|20|

## Rules Engine {#section_llt_ddv_4fb .section}

|Description|Limit|
|:----------|:----|
|Each account can have up to 1,000 rules.|1,000|
|Each rule can include a maximum of 10 data forwarding actions.|10|
|Data forwarding performance depends on the instance of the target cloud product. If the target instance has enough performance resources, Rule Engine can provide a data forwarding capability of 1,000 QPS for a single account. RAM users share the quota of the main account. A maximum of 1,000 messages can be forwarded to other cloud product instances by the Rule Engine per second. RAM users share the quota of the main account. A maximum of 1,000 messages can be forwarded to other cloud product instances by the Rule Engine per second.|1,000 QPS|
|Make sure that the instance of the target cloud product is in use. If the target instance crashes or has overdue payments, invalid parameters \(such as invalid values and changed authorizations\) or incorrect configurations, data forwarding will fail.|-|
|Data forwarded by Rules Engine may be received more than once. In a distributed environment, message balancing in real time may cause a message to be sent more than once.|-|

## APIs {#section_zds_vcv_4fb .section}

|API|One tenant \(QPS\)|One IP address \(QPS\)|
|:--|:-----------------|:---------------------|
|Pub|1,600|100|
|RRpc|1000|100|
|PubBroadcast|1|100|
|Other APIs|50|100|


