# Device log {#concept_a32_x4w_f2b .concept}

IoT Platform provides device logs that you can use to monitor your devices. On the Device Log page of the IoT Platform console, you can search for specific device logs to quickly troubleshoot any errors. This topic describes the device log querying methods, log types, and reasons for errors found in logs.

## Query device logs {#section_z51_fsw_f2b .section}

Device logs can be of the following four types:

-   [Device activity analysis logs](#)
-   [Upstream data analysis logs](#)
-   [Downstream data analysis logs](#)
-   [TSL data analysis logs](#)

Note that Basic Edition products only support the following three types of logs: device activity analysis logs, upstream data analysis logs, and downstream data analysis logs. Pro Edition products support all four types of logs.

Query device logs:

1.  In the left-side navigation pane of the IoT Platform console, click **Maintenance** \> **Device Log**.
2.  Enter the target items to filter, such as product name, log type, device name, and time range, and then click **Search**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154780940935438_en-US.png)

    Filters for device logs:

    |Filter|Description|
    |:-----|:----------|
    |DeviceName|The device name, which is a unique identifier of a device in a product. You can query logs of a device by using the device name as the filter.|
    |MessageID|The message ID, which is the unique identifier of a message in IoT Platform. You can enter a message ID to search for the corresponding message forwarding process.|
    |Status|The logs that display operation results. The value can be either successful or failed. Options:    -   All
    -   Successful
    -   Failed
|
    |Time range|A specific time range you can specify for querying logs in that period.|


**Note:** 

-   In the following sections, curly braces `{}` in log content represent variables. In actual log content, the real variable is displayed.
-   Log content is in English.
-   When error logs are displayed, all errors \(except for `system errors`\) are caused by improper operations or violations of product restrictions. Such errors need to be rectified carefully.

## Device activity analysis logs {#section_aq2_ksw_f2b .section}

Device activity analysis logs include logs of devices connecting to IoT Platform \(online\) and logs of devices disconnecting from IoT Platform \(offline\).

Device activity analysis logs can be queried by device names and time ranges as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15478094096524_en-US.png)

**Device connection failures**

|Message|Description|
|:------|:----------|
|Kicked by the same device|Another device installed with the same device certificate as this device has connected to IoT Platform, and has brought this device offline.|
|Connection reset by peer|The TCP connection has been reset by the peer.|
|Connection occurs exception|A connection exception has occurred, and the IoT Platform server has closed the connection.|
|Device disconnect|The device sent a disconnection request.|
|Keepalive timeout|No package was received in a Keep Alive interval, and the IoT Platform server has closed the connection.|
|gateway offline|The gateway device of the sub-device is offline.|

## Upstream data analysis logs {#section_zpr_ksw_f2b .section}

Upstream data analysis logs indicate logs of the following processes: devices sending messages to topics, messages being forwarded to the rules engine, and the rules engine forwarding the messages to a target topic or other Alibaba Cloud services.

You can query the upstream data analysis logs by device names, message IDs, status, or time ranges, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15478094106525_en-US.png)

**Error log description**

**Note:** Error logs include the log content, error messages, and error message descriptions.

|Content|Error message|Description|
|-------|-------------|-----------|
|Device publish message to topic:\{\},QoS=\{\},protocolMessageId:\{\}|Rate limit:\{maxQps\},current qps:\{\}|The device publishes messages in a frequency that exceeds the upper limit.|
|No authorization|Not authorized.|
|System error|A system error occurred.|
|Bad Request|A parameter error has occurred. A parameter or parameters such as topic, payload, token, or option for CoAP communication, are incorrect or are missing.|
|send message to RuleEngine，topic:\{\} protocolMessageId:\{\}|\{eg，too many requests\}|Other failure reasons, for example, IoT Platform sends too many requests to the rules engine.|
|System error|A system error occurred.|
|Transmit data to DataHub,project:\{\},topic:\{\},from IoT topic:\{\}|DataHub Schema:\{\} is invalid!|Data type mismatch.|
|DataHub IllegalArgumentException:\{\}|Invalid DataHub parameters.|
|Write record to DataHub occurs error! errors:\[code:\{\},message:\{\}\]|An error occurred when data was written to DataHub.|
|Datahub ServiceException:\{\}|DataHub service exception.|
|System error|A system error occurred.|
|Transmit data to MNS,queue:\{\},theme:\{\},from IoT topic:\{\}|MNS IllegalArgumentException:\{\}|Message Service parameter exception.|
|MNS ServiceException:\{\}|Message Service exception.|
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
|Check payload, payload:\{\}|Payload is not json|The payload is not in JSON format.|

## Downstream data analysis logs {#section_mlq_lsw_f2b .section}

Downstream data analysis logs are logs about messages sent from IoT Platform to devices.

You can filter logs by device names, message IDs, status, and time ranges, as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/15478094106526_en-US.png)

**Error log description**

**Note:** The logs include the log contents, error messages, and error message descriptions.

|Content|Error message|Description|
|-------|-------------|-----------|
|Publish message to topic:\{\},protocolMessageId:\{\}|No authorization|Not authorized.|
|Publish message to device,QoS=\{\}|IoT Hub cannot publish messages|If the IoT Platform server does not receive PUBACK from the device, it continues to send messages. When the number of messages reaches 50, the throttling policy is triggered. Consequently, IoT Platform cannot send new messages to the device.|
|Device cannot receive messages|The device client failed to receive messages. This error may be caused by slow network transmission speeds, or because the device client cannot handle any more messages.|
|Rate limit:\{maxQps\},current qps:\{\}|The frequency exceeds the upper limit.|
|Publish RRPC message to device|IoT hub cannot publish messages|The device did not respond to the server, so the server continued to send messages until it reached the frequency limit. Consequently, the server cannot send new messages.|
|Response timeout|The device has not responded to the server within the specified timeout period.|
|System error|A system error occurred.|
|Rrpc finished|\{e.g rrpcCode\}|Error messages such as UNKNOW, TIMEOUT, OFFLINE and HALFCONN are displayed.|
|Publish offline message to device|Device cannot receive messages|The device cannot receive messages from IoT Platform. The reason may be that the network condition is not stable, or the device client cannot handle any more messages.|

## TSL data analysis logs {#section_jrk_wgz_nfb .section}

TSL data analysis logs include logs of devices reporting properties and events, property settings, service callings, and the replies to property and service calls.

You can filter logs by device names and time ranges. If the device data type is Alink JSON, the page is displayed as shown in the following figure.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154780941014259_en-US.png)

If the device data type is Do not parse/Custom \(passthrough\), in addition to the log content, the hexadecimal raw data are also displayed. as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154780941021256_en-US.png)

|Parameter|Description|
|:--------|:----------|
|id|The message ID, which is the unique identifier of the message.|
|params|The request parameters.|
|Code|The result code.|
|method|The request method.|
|type|The type of message, which can be upstream or downstream.|
|scriptData|When the data type is Do not parse/Custom, the original data and parsed data are displayed.|
|downOriginalData|When the data type is Do not parse/Custom, the original downstream data to be parsed is displayed.|
|downTransformedData|When the data type is Do not parse/Custom, the parsed downstream data is displayed.|
|upOriginalData|When the data type is Do not parse/Custom, the original upstream data to be parsed is displayed.|
|upTransformedData|When the data type is Do not parse/Custom, the parsed upstream data is displayed.|

**Error logs of service callings and property settings**

When you call a service on the IoT Platform, the service parameters will be verified according to the definitions of the service in the TSL of the product.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|9201|The device is offline.|When the device is offline, this error is reported.|Check the device status in the IoT Platform console.|
|9200|The device is not activated yet.|The device has not been activated. When a new device connects to and reports data to IoT Platform, it is activated in IoT Platform.|Check the status of the device in the IoT Platform console.|
|6208|The device has been disabled.|The device has been disabled. You cannot call services of, or set properties for, a disabled device.|Check the status of the device in the IoT Platform console. If the device is disabled, enable the device and then try the operation again.|
|6300|The method parameter is not found when the system is verifying the parameters.|The specified identifier of service is not found in the TSL.|See the TSL of the product to which the device belongs in the IoT Platform console, and verify the identifier of the service.|
|6206|Failed to query the definition of the service.|The service is not found.|See the TSL of the product to which the device belongs, and check the definition of the service. Make sure that the definition of the service is the same as that in the TSL.|
|6200|Data parsing script is not found.|If the data type of the device is Do not parse/Custom, when you call a service, the data will be parsed by the script that you have defined. If you have not defined a parsing script for the product, this error code is displayed.|Go to the product details page in the IoT Platform console to verify whether the parsing script has been submitted. If the parsing script is ready, resubmit it and then try the call again.|
|6201|The parsing result is empty.|The parsing script runs normally, but returns an empty result. For example, the response of rawDataToProtocol is null, or the response of protocolToRawData is null or empty.|Check the script and troubleshoot the cause.|
|6207|The data format is incorrect.| This error may occur when devices report data to IoT Platform or you call services using the synchronous method.

 When you call services in the synchronous method, this error may be caused because:

 -   The format of the data returned by the device is incorrect.
-   The format of the parsed result for Do not parse/Custom data is incorrect.
-   The format of the input parameters is incorrect.

 |For data format in calling services, see [API documentations](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Overview.md#) and the TSL of the product. For the data format of Alink JSON, see [Alink protocol](../../../../../reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Alink protocol.md#).|
|**System exception codes**|
|5159|Failed to obtain the property information from the TSL.|A system exception occurred.|Open a ticket in the console and submit information about the error in the ticket for further consultation.|
|5160|Failed to obtain the event information from the TSL.|
|5161|Failed to obtain the event information from the TSL.|
|6661|Failed to query the tenant information.|
|6205|An error occurred when calling the service.|

**Error logs for reporting properties and events**

When a device is reporting a property or an event, the parameters of the property or event that you input will be verified based on the TSL of the device.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|6106|The number of properties reported exceeds the upper limit.|A device can only report up to 200 properties at a time.|View the logs of property reports and check the number of properties of the device on the IoT Platform. Or, view the local logs for the property number of the device.|
|6300|The method parameter is not found when the system is verifying the parameters.|The method parameter, which is required by the Alink protocol, is not found in the Alink \(standard\) format data reported by the device or in the parsed data of the passthrough data reported by the device.|View the logs of property reports for the reported data on the IoT Platform. Or, view the local logs for the reported data.|
|6320|The property information is not found when the system is verifying the property parameters.|The specified property is not found in the TSL of the device.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs to determine whether the specified property has already been defined. If the property has not been defined in the TSL, define it.|
|6450|The method parameter in Alink data is not found.|The parameter of method is not found in the Alink data reported by a device or in the parsed result of Do not parse/Custom data.|View the logs of device property reporting and check whether the parameter of method has been reported. Or you can check the local device logs for the information.|
|6207|The data format is incorrect.| This error occurs when you call a service in the synchronous method or devices report data to IoT Platform.

 When devices report data to IoT Platform, this error may occur because the Alink data reported by devices is not in JSON format, or the parsed result of Do not parse/Custom is not in JSON format.

 |For data format, see [Alink protocol documentations](../../../../../reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Alink protocol.md#)|
|**System exceptions**|
|6452|Traffic limiting|Traffic throttling has been triggered because too many requests have been submitted.|Open a ticket in the console for troubleshooting.|
|6760|The storage quota of the tenant is exceeded.|A system exception occurred.|Open a ticket in the console and submit information about the error in the ticket for further consultation.|

**The reply messages of service callings and property settings**

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|**Common error codes**|
|460|Invalid parameters.|The request parameters are invalid.|Open a ticket in the console for troubleshooting.|
|500|A system exception occurred.|An unknown exception occurred in the system.|Open a ticket in the console for troubleshooting.|
|400|An error occurred when calling the service.|An unknown exception occurred when calling the service.|Open a ticket in the console for troubleshooting.|
|429|Too many requests in the specified time period.|Traffic throttling has been triggered because too many requests have been submitted.|Open a ticket in the console for troubleshooting.|
|**System exception codes**|
|6452|Traffic limiting|Traffic throttling has been triggered because too many requests have been submitted.**Note:** If the data type of the device is Do not parse/Custom, you may receive this error code. The input parameters will be verified again based on the TSL of the device.

|Open a ticket in the console for troubleshooting.|

**Common error codes about TSL**

When a service of a device is being called or a device is reporting a property or an event, the input parameters of the service, property, or event will be verified based on the TSL of the device.

|Error code|Description |Cause|Troubleshooting method|
|----------|------------|-----|----------------------|
|6321|The identifier of the property is not found in the TSL.|A system exception occurred.|Open a ticket in the console and submit information about the error in the ticket for further consultation.|
|6317|The TSL of the device product is incorrect.|A system exception occurred.|Open a ticket in the console and submit information about the error in the ticket for further consultation.|
|6302|Required parameters are not found.|When verifying the input parameters of the service, the system does not find one or more required parameters in the request.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs for the required parameters. Check the parameters in the TSL and make sure that you have input all the required parameters.|
|6306|The input parameter does not comply with the integer data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type in the TSL.|
|6307|The input parameter does not comply with the 32-bit floating point data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL, and the value is in the value range defined in the TSL.|
|6322|The input parameter does not comply with the 64-bit floating point data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the value is in the value range defined in the TSL.|
|6308|The input parameter does not comply with the boolean data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input parameter value is not in the range defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type in the TSL.|
|6309|The input parameter does not comply with the enum data specification defined in the TSL.|The data type of the input parameter is different from the data type defined in the TSL.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6310|The input parameter does not comply with the text data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The length of the input data exceeds the length limit defined in the TSL.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL and the data length does not exceed the limit.|
|6311|The input parameter does not comply with the date data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The input data is not a UTC timestamp.

|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type defined in the TSL.|
|6312|The input parameter does not comply with the struct data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The data type of the input parameter is different from the data type defined in the TSL.
-   The number of the struct data type parameters that you have input is different from the number of struct parameters defined in TSL.

 |On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type in the TSL.|
|6304|The input parameter is not found in the defined struct parameters in the TSL.|When the parameters are verified according to the TSL, one or more input struct parameters are not found in the defined struct parameters in the TSL.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and make sure that the data type that you have input is the same as the data type in the TSL.|
|6324|The input parameter does not comply with the array data specification defined in the TSL.|When the parameters are verified according to the TSL, the following errors may be found:-   The element data type that you input is different from the element type defined in the TSL.
-   The number of array type parameters that you have input exceeds the limit of array type parameters defined in the TSL.

| -   On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, and check the array type parameters.
-   View the upstream logs of the device, and check the number of array type elements in the data reported by the device.

 |
|6328|The input parameter is not an array type data.|When the parameters are verified according to the TSL, an input value of array type parameter is not of array type data.|On the product details page in the IoT Platform console, view the TSL of the product to which the device belongs, check the array type parameters in the TSL, and then check whether or not the parameter that you have input is of array type data.|
|6325|The element type of array type data is not supported by IoT Platform.|This error is reported when the parameters that you input according to the TSL are being verified. Currently, only the following element types of array type data are supported: int32, float, double, text, and struct.|Make sure that the element type that you have input is supported by IoT Platform.|
|**System exception codes**|
|6318|A system exception occurred when parsing the TSL.|A system exception occurred.|Open a ticket in the console and submit information about the error in the ticket for further consultation.|
|6329|Failed to parse the data of the array type data specification in the TSL when verifying the parameters.|
|6323|The parameter type of the TSL is incorrect.|
|6316|An error occurred when parsing the parameters in the TSL.|
|6314|The data type is not supported.|
|6301|An error occurred when verifying the input parameter type according to the TSL.|
|**Data parsing errors**|
|26010|Traffic throttling has been triggered because too many requests have been submitted.|Too many requests in the specified time period.|Open a ticket in the console for troubleshooting.|
|26001|The content of the parsing script is empty.|The parsing script content is not found.|On the product details page in the IoT Platform console, check your data parsing script. Make sure that the script has been saved and submitted. A draft script cannot be used to parse data.|
|26002|An exception occurred when running the script.|The script runs properly, however, the script content is incorrect, for example, there are syntax mistakes in the script.|In the IoT Platform console, enter the same parameters and run the script to debug. The console only has a basic script running environment. Therefore, it cannot precisely verify the content of the script. We recommend that you inspect your script carefully before you submit it.|
|26006|The required method is not found in the script.|The script runs properly, however, the script content is incorrect. protocolToRawData and rawDataToProtocol are required in a script. If they are not found, this error will be reported.|On the product details page in the IoT Platform console, check that protocolToRawData and rawDataToProtocol have been defined.|
|26007|The returned data type is incorrect after data parsing.|The script runs properly, but the returned result data type is incorrect. Check the definitions of protocolToRawData and rawDataToProtocol. The result data of protocolToRawData must be byte\[\] array, and the result data of rawDataToProtocol must be jsonObj \(JSON object\). If the defined result data types are not these two types, this error will be returned. After a device reports data, the execution result will be returned to the device. The returned result data also will be parsed. If you have not defined protocolToRawData in the script, the returned data may be incorrect.|Inspect the script in the IoT Platform console. Enter the input parameters, run the script, and verify whether the result data type is correct.|

## Query message content {#section_zh3_hvy_pfb .section}

Click **Message Query** and then query the payload contents that are sent by devices.

Search for payload contents by message IDs. Currently, only messages with QoS 1 can be queried.

You can select to display the original data or the Base64-encoded data.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7530/154780941021104_en-US.png)

