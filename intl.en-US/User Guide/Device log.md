# Device log {#concept_a32_x4w_f2b .concept}

This topic describes the four types of device logs and corresponding log details.

## Introduction {#section_z51_fsw_f2b .section}

Device logs can be of the following four types:

-   [Device activity analysis logs](#)
-   [Upstream data analysis logs](#)
-   [Downstream data analysis logs](#)
-   [TSL data analysis logs](#)

Note that Basic Edition products only support the following three types of logs: device activity analysis logs, upstream data analysis logs, and downstream data analysis logs. Pro Edition products support all four types of logs.

You can search for logs using the following filters:

|Filter|Description|
|------|-----------|
|DeviceName|Device name, which is the unique identifier of a device in a product. You can query logs of a device by using the device name as the filter.|
|MessageID|Message ID, which is the unique identifier of a message on IoT Platform. You can enter a message ID to search for the message forwarding process.|
|Status|Logs that display the operation results. The value can be either successful or failed.|
|Time range|You can specify a time range to search for logs in that period.|

**Note:** 

-   `{}` in this documentation indicates a variable. In an actual log, this indicates actual information of your devices.
-   Logs are generated in English only.
-   When logs about failures are displayed, all errors except for `system errors` are caused by improper operations or violations of product restrictions. Such errors need to be rectified carefully.

## Device activity analysis logs {#section_aq2_ksw_f2b .section}

Device activity analysis logs include logs of devices connecting to IoT Platform \(online\) and logs of devices disconnection from IoT Platform \(offline\).

You can filter logs by device name and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15450398136524_en-US.png)

**Device connection failures**

|Message|Description|
|-------|-----------|
|Kicked by the same device|Another device installed with the same ProductKey and DeviceName of this device has connected to IoT Platform, which means this device is kicked off.|
|Connection reset by peer|The TCP connection has been reset by the peer.|
|Connection occurs exception|Connection exception. The IoT server disconnected itself.|
|Device disconnect|The device sent a MQTT disconnection request.|
|Keepalive timeout|Keepalive has timed out, and the IoT server has disconnected.|

## Upstream data analysis logs {#section_zpr_ksw_f2b .section}

Upstream data analysis logs indicate logs of the following processes: devices sending messages to topics; messages being forwarded to rules engine; and rules engine forwarding the messages to a cloud service.

You can filter logs by device name, message ID, status, and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15450398136525_en-US.png)

**Upstream data analysis log description**

**Note:** Upstream data analysis logs include the log contents, error messages \(if the operations failed\), and error message descriptions.

|Content|Error message|Cause|
|-------|-------------|-----|
|Device publish message to topic:\{\},QoS=\{\},protocolMessageId:\{\}|Rate limit:\{maxQps\},current qps:\{\}|The device publishes messages in a frequency that exceeds the upper limit.|
|No authorization|No authorization|
|System error|A system error occurred.|
|send message to RuleEngine，topic:\{\} protocolMessageId:\{\}|\{eg，too many requests\}|Other failure reasons, for example, IoT hub sends too many requests.|
|System error|A system error occurred.|
|Transmit data to DataHub,project:\{\},topic:\{\},from IoT topic:\{\}|DataHub Schema:\{\} is invalid!|Data type mismatch.|
|DataHub IllegalArgumentException:\{\}|Parameter exception.|
|Write record to DataHub occurs error! errors:\[code:\{\},message:\{\}\]|An error occurred when data was written to DataHub.|
|Datahub ServiceException:\{\}|DataHub service exception.|
|System error|A system error occurred.|
|Transmit data to MNS,queue:\{\},theme:\{\},from IoT topic:\{\}|MNS IllegalArgumentException:\{\}|Message Service parameter exception.|
|Message Service \(MNS\) ServiceException:\{\}|Message Service exception.|
|MNS ClientException:\{\}|Message Service client exception.|
|System error|A system error occurred.|
|Transmit data to MQ,topic:\{\},from IoT topic:\{\}|MQ IllegalArgumentException:\{\}|Message Queue parameter exception.|
|MQ ClientException:\{\}|Message Queue client exception.|
|System error|A system error occurred.|
|Transmit data to TableStore,instance:\{\},tableName:\{\},from IoT topic:\{\}|TableStore IllegalArgumentException:\{\}|Table Store parameter exception.|
|TableStore ServiceException:\{\}|Table Store service exception.|
|TableStore ClientException:\{\}|Table Store client exception.|
|System error|A system error occurred.|
|Transmit data to RDS,instance:\{\},databaseName:\{\},tableName:\{\},from IoT topic:\{\}|RDS IllegalArgumentException:\{\}|ApsaraDB for RDS parameter exception|
|RDS CannotGetConnectionException:\{\}|Failed to connect to ApsaraDB for RDS.|
|RDS SQLException:\{\}|SQL statement for ApsaraDB for RDS is invalid.|
|System error|A system error occurred.|
|Republish topic, from topic:\{\} to target topic:\{\}|System error|A system error occurred.|
|RuleEngine receive message from IoT topic:\{\}|Rate limit:\{maxQps\},current qps:\{\}|The frequency exceeds the upper limit.|
|System error|A system error occurred.|
|Check payload, payload:\{\}|Payload is not json|Illegal JSON format of Payload.|

## Downstream data analysis logs {#section_mlq_lsw_f2b .section}

Downstream data analysis logs are logs about messages sent from IoT Hub to devices.

You can filter logs by device name, message ID, status, and time range, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15450398136526_en-US.png)

**Downstream data analysis log**

**Note:** Downstream data analysis logs include the contents, error messages \(if the operations failed\), and error message descriptions.

|Content|Error message|Cause|
|-------|-------------|-----|
|Publish message to topic:\{\},protocolMessageId:\{\}|No authorization|No authorization.|
|Publish message to device,QoS=\{\}|IoT hub cannot publish messages|The server did not receive a PubAck message from the device. Therefore, it continues to send messages until the frequency reaches the threshold of 50 QPS.|
|Device cannot receive messages|The device failed to receive messages or the server failed to send messages. This error may be because of slow network transmission speeds, or because the device client cannot handle any more messages.|
|Rate limit:\{maxQps\},current qps:\{\}|The upper limit of the frequency has been reached.|
|Publish RRPC message to device|IoT Hub cannot publish messages|The device did not respond to the server. Therefore, the server continued to send messages until it reached the frequency limit. Consequently, the server cannot send new messages.|
|Response timeout|The device has not responded to the server within the specified timeout period.|
|System error|A system error occurred.|
|RRPC finished|\{e.g rrpcCode\}|Error messages such as UNKNOW, TIMEOUT, OFFLINE and HALFCONN.|
|Publish offline message to device|Device cannot receive messages|The device client failed to receive messages or the server failed to send messages. This error may be because of slow network transmission speeds, or because the device client cannot handle any more messages.|

## TSL data analysis logs {#section_jrk_wgz_nfb .section}

TSL data analysis logs include logs of devices reporting properties and events, property settings, service callings, and the replies to property and service callings.

You can filter logs by device name and time range. If the device data type is Alink JSON, the page is displayed as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154503981314259_en-US.png)

If the device data type is Do not parse/Custom \(passthrough\), in addition to the log content, the hexadecimal raw data are also displayed. See the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154503981421256_en-US.png)

**Error logs of service callings and property settings**

When you call a service on the IoT Platform, the service parameters will be verified according to the definitions of the service in the TSL of the product.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|9201|The device is offline.|When the device is offline, this error occurs.|Check the device status in the IoT Platform console.|
|9200|The device is not activated yet.|The device has not been activated on IoT Platform. A new device must report data to IoT Platform to be activated.|You can check the status of the device in the IoT Platform console.|
|6208|The device has been disabled.|The device has been disabled. You cannot call services of, or set properties for, a disabled device.|You can check the status of the device in the IoT Platform console. You can enable the device and then try the operation again.|
|6300|The specified value of method is not found when verifying the input parameters according to the TSL.|The specified identifier of service is not found in the TSL.|See the TSL of the product to which the device belongs in the IoT Platform console, and verify the identifier of the service.|
|6206|Failed to query the definition of the service.|The service is not found.|See the TSL of the product to which the device belongs, and check the definition of the service. Make sure that the definition of the service is the same as that in the TSL.|
|6200|Data parsing script is not found.|If the data type of the device is Do not parse/Custom, when you call a service, the data will be parsed by the script that you have defined. If you have not defined a parsing script for the product, this error code is displayed.|Go to the product details page in the IoT Platform console to verify whether the parsing script has been submitted. If the parsing script is ready, submit it again and then try the call again.|
|6201|The parsing result is empty.|The parsing script runs normally, but returns an empty result. For example, the response of rawDataToProtocal is null, or the response of protocalToRawData is null or empty.|Check the script and troubleshoot the cause.|
|**System exception codes**|
|5159|Failed to obtain the property information from the TSL.|System exceptions|Open a ticket in the console and submit information about the error in the ticket for further consultation.|
|5160|Failed to obtain the event information from the TSL.|
|5161|Failed to obtain the event information from the TSL.|
|6661|Failed to query the tenant information.|
|6205|Failed to call message-broker.|

**Devices report properties and events**

When a service of a device is being called or a device is reporting a property or an event, the parameters of the service, property, or event that you input will be verified based on the TSL of the device.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|6106|The number of properties reported exceeds the upper limit.|A device can report up to 200 properties at a time.|View the logs of property reports and check the number of properties of the device on the IoT Platform. Or, view the local logs for the property number of the device.|
|6300|The method parameter is not found when the system is verifying the parameters.|The method parameter, which is required by the Alink protocol, is not found in the Alink \(standard\) format data reported by the device or in the parsed data of the passthrough data reported by the device.|View the logs of property reports for the reported data on the IoT Platform. Or, view the local logs for the reported data.|
|6320|The property information is not found when the system is verifying the property parameters.|The specified property is not found in the TSL of the device.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs to determine whether the specified property has already been defined. If the property has not been defined in the TSL, define it.|
|**System exception codes**|
|6452|Traffic limiting|The traffic throttling has been triggered because too many requests have been submitted.|Open a ticket in the console for troubleshooting.|
|6760|The storage quota of the tenant is exceeded.|A system exception occurred.|Open a ticket in the console for troubleshooting.|

**The reply messages of service callings and property settings.**

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|**Common error codes**|
|460|Parameter error|The request parameters are invalid.|Open a ticket in the console for troubleshooting.|
|500|Internal system error|An unknown exception occurred in the system.|Open a ticket in the console for troubleshooting.|
|400|An error occurred when calling the service.|An unknown exception occurred when calling the service.|Open a ticket in the console for troubleshooting.|
|**System exception codes**|
|6452|Traffic limiting|The traffic throttling has been triggered because too many requests have been submitted.**Note:** If the data type of the device is Do not parse/Custom, you may receive this error code. The input parameters will be verified again based on the TSL of the device.

|Open a ticket in the console for troubleshooting.|

**Common error codes about TSL**

When a service of a device is being called or a device is reporting a property or an event, the input parameters of the service, property, or event will be verified based on the TSL of the device.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|6321|The identifier of the property is not found in the TSL.|A system exception occurred.|Open a ticket in the console for troubleshooting.|
|6317|The TSL of the device product is incorrect.|A system exception occurred.|Open a ticket in the console for troubleshooting.|
|6302|Required parameters are not found.|When verifying the input parameters of the service, the system does not find one or more required parameters in the request.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs. Check the parameters in the TSL and make sure that you have input all the required parameters.|
|6306|The input parameter does not comply with the integer data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the value is in the value range defined in the TSL.|
|6307|The input parameter does not comply with the 32-bit float data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the value is in the value range defined in the TSL.|
|6322|The input parameter does not comply with the 64-bit float data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the value is in the value range defined in the TSL.|
|6308|The input parameter does not comply with the boolean data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6309|The input parameter does not comply with the enum data specification defined in the TSL.|The data type of the input parameter is different from the data type defined in the TSL.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6310|The input parameter does not comply with the text data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The length of the input data exceeds the length limit defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the data length does not exceed the limit.|
|6311|The input parameter does not comply with the date data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input data is not a UTC timestamp.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6312|The input parameter does not comply with the struct data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   Specifically, the number of the struct data type parameters that you have input is different from the number of struct parameters defined in TSL.

 |On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6304|The input parameter is not found in the defined struct parameters in the TSL.|When the parameters are verified according to the TSL, an input struct parameter is not found in the defined struct parameters in the TSL.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type in the TSL.|
|6324|The input parameter does not comply with the array data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The element data type that you input is different from the element type defined in the TSL.
-   Specifically, the number of array type parameters that you have input exceeds the number of array type parameters defined in the TSL.

| -   On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and check the array type parameters.
-   View the upstream logs of the device, and check the number of array type elements in the data reported by the device.

 |
|6328|The input parameter is not an array type data.|When the parameters are verified according to the TSL, an input value of array type parameter is not an array type data.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, check the array type parameters in the TSL, and then check whether or not the parameter that you have input is an array type data.|
|6325|The element type of array type data is not supported by IoT Platform.|An error is reported when verifying the parameters that you input according to the TSL. Currently, only the following element types of array type data are supported: int32, float, double, text, and struct.|Make sure that the element type that you have input is supported by IoT Platform.|
|**System exception codes**|
|6318|A system exception occurred when parsing the TSL.|A system exception occurred.|Open a ticket in the console for troubleshooting.|
|6329|Failed to parse the data of the array type data specification in the TSL when verifying the parameters.|
|6323|The parameter type of the TSL is incorrect.|
|6316|An error occurred when parsing the parameters in the TSL.|
|6314|The data type is not supported.|
|6301|An error occurred when verifying the input parameter type according to the TSL.|
|**Data parsing script error codes**|
|26010|Traffic throttling is triggered because too many requests are submitted.|Too many requests in a short time.|Open a ticket in the console for troubleshooting.|
|26001|The content of the parsing script is empty.|The parsing script content is not found.|On the product details page in the IoT Platform console, check your data parsing script. Make sure that the script has been saved and submitted. Note that a draft script cannot be used when verifying parameters according to the TSL.|
|26002|An exception occurred when running the script.|The script runs properly, however, the script content is incorrect, for example, there are syntax mistakes in the script.|In the IoT Platform console, enter the same parameters and run the script to debug. The console only has a basic script running environment. Therefore, it cannot verify the script across every detail. We recommend that you inspect your script carefully before you submit it.|
|26006|The required method is not found in the script.|The script runs properly, however, the script content is incorrect. protocalToRawData and rawDataToProtocal are required in a script. If they are not found, this error will be reported.|On the product details page in the IoT Platform console, check that protocalToRawData and rawDataToProtocal have been defined.|
|26007|The returned data type is incorrect after data parsing.|The script runs properly, but the returned result data type is incorrect. Check the definitions of protocalToRawData and rawDataToProtocal. The result data of protocalToRawData must be byte\[\] array, and the result data of rawDataToProtocal must be jsonObj \(JSON object\). If the defined result data types are not these two types, this error will be returned. After a device reports data, the execution result will be returned to the device. The returned result data also will be parsed. If you have not defined protocolToRawData in the script, the returned data may be incorrect.|Inspect the script in the IoT Platform console. Enter the input parameters, run the script, and verify whether the result data type is correct.|

## Query messages {#section_zh3_hvy_pfb .section}

You can use **Message Query** to query the payload contents that are sent by devices.

Search for payload contents by message IDs. Currently, only messages that are sent in QoS 1 are supported. See the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154503981421104_en-US.png)

Enter a message ID and click Search. The payload content of the message will be displayed. You can select the displayed content data type from the two types: Base64-encoded Data and Raw Data.

