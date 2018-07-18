# Log service {#concept_a32_x4w_f2b .concept}

This topic describes three types of Log Service and log details.

## Usage {#section_z51_fsw_f2b .section}

There are three types of logs:

-   [Device behavior analytics](#section_aq2_ksw_f2b)
-   [Upstream analytics](#section_zpr_ksw_f2b)
-   [Downstream analytics](#section_mlq_lsw_f2b)

This following table describes the methods for using IoT Platform to filter logs.

|Filter method|Description|
|-------------|-----------|
|DeviceName|Specifies the device name. It is the unique identifier of a device for a product. You can filter logs by deviceName.|
|MessageId|Specifies the message ID. It is the unique identifier of a message on IoT Platform. You can use the messageId to track the entire process of message forwarding.|
|Status|Log service has two statuses: success and failure.|
|Time range|Filters logs based on the time range specified.|

**Note:** 

-   `{}` indicates variables. The system will display logs based on the actual running.
-   Logs are in English only.
-   When logs about failures are displayed, all errors except `system error` are caused by improper use or violations of product restrictions. These errors need to be rectified carefully.

## Device behavior analytics {#section_aq2_ksw_f2b .section}

Device behavior analytics includes the analytics of the online and offline logs for a device.

You can filter logs by DeviceName and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/6524_en-US.png)

**Device connection failure causes **

|Detail| Description|
|------|------------|
|Kicked by the same device| Other device used the same ProductKey, DeviceName and ProductKey to get online, and this device is kicked off.|
|Connection reset by peer| TCP connection is reset by peer.|
|Connection occurs exception| Connection exception. IoT server disconnected itself.|
|Device disconnect| Device sent MQTT disconnection request.|
|Keepalive timeout| Keepalive timeout. IoT server disconnected.|

## Upstream analytics {#section_zpr_ksw_f2b .section}

Upstream analytics indicates the analytics of the following processes: A device sends messages to a topic; the topic forwards the messages to rules engine; rules engine forwards the messages to a cloud service.

You can filter logs by DeviceName, MessageId, status, and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/6525_en-US.png)

**Upstream analytics \(English and Chinese\)**

**Note:** Upstream analytics includes the context, failure of causes, and cause descriptions.

|Context|Cause of failure|Cause description|
|Device publish message to topic:\{\},QoS=\{\},protocolMessageId:\{\}|Rate limit:\{maxQps\},current qps:\{\}|Restriction violations|
|No authorization|No authorization|
|System error|System error|
|send message to RuleEngine，topic:\{\} protocolMessageId:\{\}|\{eg，too many requests\}|Causes of connection failure, for example, too many requests for query.|
|System error|System error|
|Transmit data to DataHub,project:\{\},topic:\{\},from IoT topic:\{\}|DataHub Schema:\{\} is invalid!|Data type mismatch|
|DataHub IllegalArgumentException:\{\}|Parameter exception|
|Write record to DataHub occurs error! errors:\[code:\{\},message:\{\}\]|An error that occurs when data is written to DataHub|
|Datahub ServiceException:\{\}|DataHub exception|
|System error|System errors|
|Transmit data to MNS,queue:\{\},theme:\{\},from IoT topic:\{\}|MNS IllegalArgumentException:\{\}| MNS parameter exception|
|Message Service \(MNS\) ServiceException:\{\}|MNS service exception|
|MNS ClientException:\{\}|MNS client exception|
|System error|System error|
|Transmit data to MQ,topic:\{\},from IoT topic:\{\}|MQ IllegalArgumentException:\{\}|MQ parameter exception|
|MQ ClientException:\{\}|Message Queue \(MQ\) client exception|
|System error|System error|
|Transmit data to TableStore,instance:\{\},tableName:\{\},from IoT topic:\{\}|TableStore IllegalArgumentException:\{\}|Table Store parameter exception|
|TableStore ServiceException:\{\}|Table Store exception|
|TableStore ClientException:\{\}|Table Store client exception|
|System error|System error|
|Transmit data to RDS,instance:\{\},databaseName:\{\},tableName:\{\},from IoT topic:\{\}|RDS IllegalArgumentException:\{\}|RDS parameter exception|
|RDS CannotGetConnectionException:\{\}|RDS failure of connecting to IoT Hub|
|RDS SQLException:\{\}|RDS SQL statement exception|
|System error|System error|
|Republish topic, from topic:\{\} to target topic:\{\}|System error|System error|
|RuleEngine receive message from IoT topic:\{\}|Rate limit:\{maxQps\},current qps:\{\}|Restriction violations|
|System error|System error|
|Check payload, payload:\{\}|Payload is not json|Illegal JSON format of Payload|

## Downstream analytics {#section_mlq_lsw_f2b .section}

Downstream analytics are the logs about messages sent from IoT Hub to your device.

You can filter logs by DeviceName, MessageId, execution status, and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/6526_en-US.png)

**Downstream analytics**

**Note:** Downstream analytics includes the context, causes of failure, and cause descriptions.

|Context|Cause of failure|Cause description|
|Publish message to topic:\{\},protocolMessageId:\{\}|No authorization|No authorization|
|Publish message to device,QoS=\{\}|IoT hub cannot publish messages|The server keeps sending messages until QPS reaches the threshold of 50, because it does not receive puback packets from the device. |
|Device cannot receive messages|The device fails to receive messages or the server fails to send messages possibly due to the slow network transmission speed, or because the server QPS has reached its limit.|
|Rate limit:\{maxQps\},current qps:\{\}|Restriction violations|
|Publish RRPC message to device|IoT Hub cannot publish messages|The device does not respond to the server, but the server keeps sending messages until its QPS reaches its limit. Consequently, the server fails to send new messages.|
|Response timeout|Response timeout|
|System error|System error|
|RRPC finished|\{e.g rrpcCode\}|Printed RRPCCode such as UNKNOW, TIMEOUT, OFFLINE and HALFCONN.|
|Publish offline message to device|Device cannot receive messages|The device fails to receive messages or the server fails to send messages possibly due to the slow network transmission speed, or because the server QPS has reached its limit.|

