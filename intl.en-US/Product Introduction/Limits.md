# Limits {#concept_gyg_xrt_tdb .concept}

The following table describes the limits of IoT Platform:

|Limit|Description|
|:----|:----------|
|Total products|Each account can create up to 1,000 products.|
|Total devices|Each product can include up to 500,000 devices.|
|Gateways and sub-devices| -   Each gateway can include up to 1,500 sub-devices.
-   Each gateway can have up to 1,000 sub-device channels.

 |
|Total tags|Each product, device, or device group can have up to 100 tags.|
|Parsing scripts|The size of the parsing script cannot exceed 48 KB.|
|Remote configuration|The remote configuration file can only be in JSON format and cannot exceed 64 KB.|
|Features \(only for IoT Platform Pro products\)|Each product can have up to 100 features.|
|When you define a property, the number of parameters for the struct data cannot exceed 10.|
| If the feature data type is:

 -   enum, the number of enumeration items cannot exceed 25.
-   text, the data cannot exceed 1024 bytes.
-   array, the number of elements in an array cannot exceed 128.

 |
|Each service can have a maximum of 10 input parameters and 10 output parameters.|
|Each event can have a maximum of 10 output parameters.|
|Topic categories|A product can define a maximum of 50 topic categories.|
|Broadcast topic|A maximum of 1,000 devices can subscribe to the same broadcast topic. The SDKs in the server can only send one broadcast per second.|
|Topic length|A topic cannot exceed 128 bytes.|
|Protocol package size|The CoAP protocol package cannot exceed 1 KB.|
|The MQTT protocol package cannot exceed 256 KB.|
|Communications|A device can only publish and subscribe to its own topic.|
|Topic subscription| After you subscribe to or unsubscribe from a topic, the change will take effect in 10 seconds. Subscriptions remain in effect until you unsubscribe from the topic. We recommend that you subscribe to topics in advance to avoid missing information.

 Example: A device sends a SUB request to topic A. After 10 seconds, the subscription takes effect and the device starts to receive messages in real time. The device will continue receiving messages from topic A until you unsubscribe from it.

 |
|Rule Engine|Each account can set up to 1,000 rules.|
|Each rule can include a maximum of 10 data forwarding actions.|
|Data forwarding performance depends on the target instance of the cloud product. If the target instance has enough performance resources, Rule Engine can provide a data forwarding capability of 1,000 QPS for a single account. If the request quantity exceeds this limit, or the target instance cannot write in the data in 1 second, data flow forwarding will be restricted.|
|Make sure that the target instance of the cloud product is in use. If the target instance crashes or has overdue payments, invalid parameters \(such as invalid values and changed authorizations\) or incorrect configurations, data flow forwarding will fail.|
|Throttling|Data per device:-   Upload speed: 30 messages/second in QoS0 level; 10 messages/second in QoS1 level.
-   Download speed: 50 messages/second.
-   Each device has a maximum download bandwidth of 512 KB. If the TCP write buffer is blocked, an error is returned. For example, if you use the Pub operation to send requests to a device, but the device cannot handle the amount of requests, you will receive an error due to throttling.

**Note:** If you use the MQTT protocol to connect to your devices and IoT Platform, you cannot receive error messages resulting from throttling. However, you can view logs to detect which devices are being throttled.


Cloud-side API limits:

-   For a single IP address, the limit for API calling is 100 QPS.
-   For a single Alibaba Cloud account, there are different calling limits for different APIs. For RRPC, the limit is 500 QPS; for Pub, the limit is 1,500 QPS; and for other APIs, if not specified, the default limit is 50 QPS.

**Note:** 

-   If any of the limits mentioned in this document affect your business requirements, please submit a ticket and describe your business requirements for IoT Platform.
-   If you are calling an API and you receive an error \(such as 29-31 in [Common errors](https://error-center.aliyun.com/status/product/Public?spm=5176.10421674.home.64.319a2fcfOdrQxD)\), wait and then try the call again.

|

