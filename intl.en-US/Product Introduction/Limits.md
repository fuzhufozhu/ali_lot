# Limits {#concept_gyg_xrt_tdb .concept}

The following table shows the limits for the IoT Platform:

|Limit|Description|
|:----|:----------|
|Total products|Each account can create up to 1,000 products.|
|Total devices|Each product can include up to 0.5 million devices.|
|Parsing scripts|The size of the parsing script cannot exceed 48 KB.|
|Remote configuration|The remote configuration file only supports the JSON format and cannot exceed 64 KB.|
|Features \(IoT Platform Pro products only\)|Each product can add up to 100 features.|
|When you define a property, the number of parameters for the struct data cannot exceed 10.|
| If the feature data type is:

 -   enum, the number of enumeration items cannot exceed 25.
-   text, the data cannot exceed 1024 bytes.
-   array, the number of elements in an array cannot exceed 128.

 |
|Each service can have a maximum of 10 input parameters and 10 output parameters.|
|Each event can have a maximum of 10 output parameters.|
|Topic categories|A product can define a maximum of 50 topic categories.|
|Broadcast topic|The same broadcast topic can be subscribed by a maximum of 1000 devices. The SDKs in the server can only send one broadcast per second.|
|Topic length|Cannot exceed 128 bytes.|
|Protocol package size|The CoAP protocol package cannot exceed 1 KB.|
|The MQTT protocol package cannot exceed 256 KB.|
|Communications|The device can only publish and subscribe to its own topic.|
|Topic subscription| After you subscribe to or unsubscribe from a topic, the change will take effect in 10 seconds. Subscriptions remain in effect until you unsubscribe from the topic. We recommend that you subscribe to Topics in advance to avoid missing information.

 Example: The device sends a SUB request to topic A. After 10 seconds, the subscription takes effect and the device starts to receive messages in real time. The device will continue receiving messages for topic  A until you unsubscribe from it.

 |
|Rule Engine|Each account can set up to 100 rules.|
|Each rule can include a maximum of 10 actions related to data transfer.|
|Throttling|Data per device:-   The upload speed: 30 messages/second in QoS0 level, 10 messages/second in QoS1 level.
-   The download speed: 50 messages/second.

|

