# Error codes for device SDKs {#concept_354639 .concept}

This topic describes error codes for device SDKs.

## Common error codes {#section_1wh_1mc_k6e .section}

|Error code|Cause|Solution|
|:---------|:----|:-------|
|400|An error occurred while processing the request.|Submit a ticket.|
|429|Traffic throttling is triggered due to frequent requests.|Submit a ticket.|
|460|The data reported by the device is empty, the format of the parameters is invalid, or the number of parameters has reached the upper limit.|Follow the data formats described in [Communications over Alink protocol](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Communications over Alink protocol.md#).|
|500|An unknown error occurred in the system.|Submit a ticket.|
|5005|An error occurred while querying the product information.|Check the product information in the console and make sure that the ProductKey is correct.|
|5244|An error occurred while querying the metadata of LoRaWAN-based products.|Submit a ticket.|
|6100|An error occurred while querying the information about the specified device.|Log on to the console and check whether the device information on the Devices page is correct.|
|6203|An error occurred while parsing the topic.|Submit a ticket.|
|6250|An error occurred while querying the product information.|Check the product information in the console and make sure that the ProductKey is correct.|
|6204|The specified device is disabled. You cannot perform any operation on this device.|Check the status of the device on the Devices page in the console.|
|6450|The method parameter is missing after the pass-through \(custom\) data is parsed to the standard Alink format.|On the Device Log page in the console, or in the local log file of the device, check whether the data reported by the device contains the method parameter.|
|6760|An error occurs in the system.|Submit a ticket.|

|Error code|Cause|Solution|
|:---------|:----|:-------|
|26001|The system does not find any parsing script.|Navigate to the Data Parsing tab in the console and make sure that the script has been submitted. **Note:** You cannot run scripts that are not submitted.

 |
|26002|The script runs correctly, but it contains errors.|Use the same data to test the script. Check the error message and revise the script. We recommend that you test the script on a local device before you submit the script to IoT Platform.|
|26006|The script runs correctly but it contains errors. The script must contain the protocolToRawData and rawDataToProtocol methods. An error occurs if these methods are missing.|Navigate to the Data Parsing tab in the console and check whether the protocolToRawData and rawDataToProtocol methods exist.|
|26007|The script runs correctly, but the format of the response is invalid. The script must contain the protocolToRawData and rawDataToProtocol methods. The protocolToRawData method must return a byte\[\] array, and the rawDataToProtocol method must return a JSON object. An error occurs if the response is not in the required format.|Test the script in the console or on a local device, and check whether the format of the responses returned by these methods is valid.|
|26010|Traffic throttling is triggered due to frequent requests.|Submit a ticket.|

|Error code|Cause|Solution|
|----------|-----|--------|
|5159|When the system verifies parameters based on the TSL model, an error occurred while querying the property.|Submit a ticket.|
|5160|When the system verifies parameters based on the TSL model, an error occurred while querying the event.|Submit a ticket.|
|5161|When the system verifies parameters based on the TSL model, an error occurred while querying the service.|Submit a ticket.|
|6207|The Alink data reported by the device or the data returned after the system parses the custom data is not in the JSON format.|Follow the data formats described in [Device properties, events, and services](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device properties, events, and services.md#) when you report data.|
|6300|The specified method does not exist. When the system verifies parameters based on the TSL model, the method parameter does exist in the Alink data reported by the device, or after the pass-through \(custom\) data is parsed to Alink format.|On the Device Log page in the console, or in the local log file of the device, check whether the data reported by the device contains the method parameter.|
|6301|When the system verifies parameters based on the TSL model, the data type is specified as an array. However, the type of data reported by the device is not an array.|Navigate to the Define Feature tab in the console, and check the data type specified in the TSL model. Report data with the required data type.|
|6302|Some required input parameters of the service are not set.|Log on to the console and check the TSL model. Make sure that the required input parameters are correctly set.|
|6306|When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameters are different from those specified in the TSL model.
-   The values of the input parameter are not within the value range specified in the TSL model.

 |Log on to the console and check the TSL model. Make sure that the data types of the input parameters are the same as those defined in the TSL model, and the parameter values are within the value range specified in the TSL model.|
|6307|The input parameter does not comply with the 32-bit float data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameter are different from those specified in the TSL model.
-   The values of the input parameters are not within the value range specified in the TSL model.

 |
|6308|The input parameters do not comply with the Boolean data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameters are different from those specified in the TSL model.
-   The values of the input parameters are not within the value range specified in the TSL model.

 |
|6310|The input parameters do not comply with the text data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the parameters are different from those specified in the TSL model.
-   The length of the parameters exceeds the upper limit specified in the TSL model.

 |
|6322|The input parameters do not comply with the 64-bit float data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameters are different from those specified in the TSL model.
-   The values of the input parameters are not within the value range specified in the TSL model.

 |
|6304|The input parameters cannot be found in the struct specified in the TSL model.|Log on to the console and check the TSL model. Make sure that the data types of the input parameters are correct.|
|6309|The input parameters do not comply with the enum data specifications specified in the TSL model.|
|6311|The input parameters do not comply with the date data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameters are different from those specified in the TSL model.
-   The input data is not a UTC timestamp.

 |
|6312|The input parameters do not comply with the struct data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The data types of the input parameters are different from those specified in the TSL model.
-   The number of the parameters contained in the struct is different from that specified in the TSL model.

 |
|6320|The specified property cannot be found in the TSL model of the device.|Log on to the console and check whether the specified property exists in the TSL model. If the property does not exist, add the property.|
|6321|The Identifier parameter of the property, event, or service is not set.|Submit a ticket.|
|6317|Parameters required in the TSL model are not set, such as the type and specs parameters.|Submit a ticket.|
|6324|The input parameters do not comply with the array data specifications specified in the TSL model. When the system verifies parameters based on the TSL model, the following errors may be found: -   The elements in the passed-in array do not match the array definition in the TSL model.
-   The number of elements in the array exceeds the upper limit specified in the TSL model.

 | -   Log on to the console and check the array definition in the TSL model.
-   View the log reported by the device and check the number of elements in the array reported by the device.

 |
|6325|The type of elements in the array is not supported by IoT Platform. Currently, the following element types are supported: int32, float, double, text, and struct.|Check whether the element type is supported by IoT Platform.|
|6326|The format of the time field reported by the device is invalid.|Follow the formats described in [Device properties, events, and services](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device properties, events, and services.md#). Report data in the required formats.|
|6328|The value of the input parameter is not an array.|Log on to the console and check the TSL model. Make sure that the data type of the input parameter is array.|
|**System error codes**|
|6318|An error occurred in the system while parsing the TSL model.|Submit a ticket.|
|6313|
|6329|
|6323|
|6316|
|6314|
|6301|

## Device registration error codes {#section_9uj_rhi_z22 .section}

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/sub/register.

Error codes: 460, 5005, 5244, 500, 6288, 6100, 6619, 6292, and 6203.

The following table lists the causes and solutions of errors that may occur when you register a device. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6288|Dynamic registration is disabled for the device.|Log on to the console and enable dynamic registration on the Product Details page.|
|6619|The device has been bound to another gateway.|Navigate to the Device Information tab in the console and check whether the sub-device has already been bound to a gateway.|

**Unique-certificate-per-product authentication**

Error codes: 460, 6250, 6288, 6600, 6289, 500, and 6292.

The following table lists the causes and solutions of errors that may occur when you dynamically register a directly connected device based on unique-certificate-per-product authentication. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6288|Dynamic registration is disabled for the device.|Log on to the console and enable dynamic registration on the Product Details page.|
|6292|The algorithm for calculating the signature is not supported by IoT Platform.|Use algorithms that are supported by the signMethod parameter, as described in [Device identity registration](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device identity registration.md#).|
|6600|An error occurred while verifying the signature.|Use the supported algorithms to calculate and verify the signature, as described in [Device identity registration](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device identity registration.md#).|
|6289|The device has already been activated.|Log on to the console and check the status of the device.|

## Topology error codes {#section_mys_8f8_vfa .section}

**Add topological relationships**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add.

Error codes: 460, 429, 6402, 6100, 401, 6204, 6400, and 6203.

The following table lists the causes and solutions of error that may occur when you add a topological relationship between the gateway and a sub-device. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|401|The system failed to verify the signature while adding the topological relationship.|Use the supported algorithms to calculate and verify the signature, as described in [Add a topological relationship](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Add a topological relationship.md#).|
|6402|The gateway and sub-device are the same device. When you add a topological relationship, you must not add the current gateway to itself as a sub-device.|View the information of all existing sub-devices, and check whether the gateway and a sub-device have the same information.|
|6400|The number of sub-devices that you have added to the gateway has reached the upper limit.|Log on to the console and check the number of existing sub-devices on the Sub-device Management tab page. For more information about the limits, see [Limits](../../../../reseller.en-US/Product Introduction/Limits.md#).|

**Delete topological relationships**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/delete.

Error codes: 460, 429, 6100, 6401, and 6203.

The following table lists the cause and solution of the error that may occur when you delete a topological relationship between the gateway and a sub-device. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6401|The topological relationship does not exist when the system verifies the topological relationship.|Log on to the console, click Devices in the left-side navigation pane, and then click the Sub-device Management tab on the Device Details page. You can then view the information about the sub-device.|

**Obtain topological relationships**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/get

Error codes: 460, 429, 500, and 6203. For more information about these error codes, see the Common error codes section in this topic.

**The gateway reports a detected sub-device**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/list/found.

Error codes: 460, 500, 6250, 6280, and 6203.

The following table lists the cause and solution of error that may occur when a gateway reports a detected sub-device. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6280|The name of the sub-device reported by the gateway is invalid. The device name can contain Chinese characters, letters, digits, and underscores \(\_\). It must be from 4 to 32 characters in length. Each Chinese character accounts for two character spaces.|Check whether the name of the sub-device reported by the gateway is valid.|

## Sub-device connection and disconnection error codes {#section_hfo_zxm_d2v .section}

**A sub-device connects to IoT Platform**

Request topic: /ext/session/$\{productKey\}/$\{deviceName\}/combine/login.

Error codes: 460, 429, 6100, 6204, 6287, 6401, and 500.

**A sub-device automatically disconnects from IoT Platform**

Error messages are sent to this topic: /ext/session/\{productKey\}/\{deviceName\}/combine/logout\_reply.

Error codes: 460, 520, and 500.

**A sub-device is disconnected from IoT Platform by force**

Error messages are sent to this topic: /ext/error/\{productKey\}/\{deviceName\}.

Error codes: 427, 521, 522, and 6401.

**A sub-devices fails to send a message**

Error messages are sent to this topic: /ext/error/\{productKey\}/\{deviceName\}.

Error code: 520.

The following table lists the causes and solutions of errors that may occur when a sub-device connects to or disconnects from IoT Platform. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|427|The device frequently reconnects to IoT Platform. The same device certificate information is used to connect another device to IoT Platform. This disconnects the current device from IoT Platform.|Navigate to the Device Details page in the console and check when the device was most recently connected to IoT Platform. You can then determine whether the same device certificate information is used to connect another device to IoT Platform.|
|428|The number of sub-devices that you have added to the specified gateway has reached the upper limit. Currently, you can add up to 1,500 sub-devices to each gateway.|Check the number of sub-devices that you have added to the gateway.|
|521|The device has been deleted.|Navigate to the Devices page in the console and check whether the device has been deleted.|
|522|The device has been disabled.|Navigate to the Devices page in the console and check the status of the device.|
|520|An error occurred with the session between the sub-device and IoT Platform. -   The specified session does not exist because the sub-device is not connected to IoT Platform, or the sub-device is already disconnected from IoT Platform.
-   The session exists, but the session is not established through the current gateway.

 |
|6287|An error occurred while verifying the signature based on the ProductSecret or DeviceSecret.|Use the supported algorithms to calculate and verify the signature, as described in [Connect and disconnect sub-devices](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Connect and disconnect sub-devices.md#).|

## Property, event, and service error codes {#section_pki_8kd_6cm .section}

**A device reports a property**

Request topic for pass-through data: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw.

Request topic for Alink data: /sys/\{productKey\}/\{deviceName\}/thing/event/property/post.

Error codes: 460, 500, 6250, 6203, 6207, 6313, 6300, 6320, 6321, 6326, 6301, 6302, 6317, 6323, 6316, 6306, 6307, 6322, 6308, 6309, 6310, 6311, 6312, 6324, 6328, 6325, 6200, 6201, 26001, 26002, 26006, and 26007.

The following table lists the cause and solution of error 6106 that may occur when the device reports a property. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6106|The number of properties that are reported by the device has reached the upper limit. A device can report up to 200 properties at the same time.|Log on to the console, choose **Maintenance** \> **Device Log**, and check the number of properties reported by the device. You can also check this information in the local log file of the device.|

**The device reports an event**

Request topic for pass-through data: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw.

Request topic for Alink Data: /sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.identifier\}/post.

Error codes: 460, 500, 6250, 6203, 6207, 6313, 6300, 6320, 6321, 6326, 6301, 6302, 6317, 6323, 6316, 6306, 6307, 6322, 6308, 6309, 6310, 6311, 6312, 6324, 6328, 6325, 6200, 6201, 26001, 26002, 26006, and 26007.

For more information about these error codes, see the Common error codes section in this topic.

**The gateway reports data of multiple sub-devices at the same time**

Request topic for pass-through data: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw.

Request topic for Alink data: /sys/\{productKey\}/\{deviceName\}/thing/event/property/pack/post.

Error codes: 460, 6401, 6106, 6357, 6356, 6100, 6207, 6313, 6300, 6320, 6321, 6326, 6301, 6302, 6317, 6323, 6316, 6306, 6307, 6322, 6308, 6309, 6310, 6311, 6312, 6324, 6328, 6325, 6200, 6201, 26001, 26002, 26006, and 26007.

The following table lists the causes and solutions of errors that may occur when the gateway reports data of multiple sub-devices at the same time. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6401|The topological relationship does not exist.|Navigate to the Sub-device Management tab in the console and check the information about the sub-device.|
|6106|The number of properties reported by the device has reached the upper limit. A device can report up to 200 properties at the same time.|Log on to the console, choose **Maintenance** \> **Device Log**, and check the number of properties reported by the device. You can also check this information in the local log file of the device.|
|6357|The amount of data reported by the gateway has reached the upper limit. When the gateway reports data for sub-devices, the gateway can report data of up to 20 devices at the same time.|Check the report records in the local log file of the device.|
|6356|The number of events reported by the gateway has reached the upper limit. When the gateway reports data for sub-devices, the gateway can report up to 200 events at the same time.|Check the report records in the local log file of the device.|

## Error codes about desired device property values {#section_8wx_pa9_mx7 .section}

**Obtain desired property values**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/property/desired/get.

Error codes: 460, 6104, 6661, and 500.

The following table lists the causes and solutions of errors that may occur when you perform operations on desired device property values. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6104|The number of properties contained in the request has reached the upper limit. A request can contain up to 200 properties.|Log on to the console, choose **Maintenance** \> **Device Log**, and check the number of properties in the reported data. You can also check this information in the local log file of the device.|
|6661|An error occurred while querying the desired property.|Submit a ticket.|

**A device clears the desired property values**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/property/desired/delete.

Error codes: 460, 6104, 6661, 500, 6207, 6313, 6300, 6320, 6321, 6326, 6301, 6302, 6317, 6323, 6316, 6306, 6307, 6322, 6308, 6309, 6310, 6311, 6312, 6324, 6328, and 6325.

## Device tag error codes {#section_u52_6mz_dak .section}

**A device reports tag information**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/update.

Error codes: 460 and 6100.

**A device deletes tag information**

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/delete.

Error codes: 460 and 500.

## Error codes about obtaining TSL models {#section_ths_4p5_eo1 .section}

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/dsltemplate/get.

Error codes: 460, 5159, 5160, and 5161.

## Error codes about querying firmware information {#section_39m_t12_bhc .section}

Request Topic: /ota/device/request/$\{YourProductKey\}/$\{YourDeviceName\}.

**Note:** 

-   In the AbstractAlinkJsonMessageConsumer class that provides methods for upgrading firmware, only the DeviceOtaUpgradeReqConsumer method returns error codes. Other methods do not return error codes or data.
-   The topic used to query firmware information is the same as that used to return responses.

Error codes: 429, 9112, and 500.

The following table lists the cause and solution of the error that may occur when a device queries firmware information. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|9112|The system failed to query information about the specified device.|Check whether the device information specified in the console is correct.|

## Error codes about obtaining the configuration information {#section_f68_53k_lcs .section}

Request topic: /sys/\{productKey\}/\{deviceName\}/thing/config/get.

Error codes: 460, 500, 6713, and 6710.

The following lists the causes and solution of errors that may occur when a device attempts to obtain the configuration information. For more information about the other error codes, see the Common error codes section in this topic.

|Error code|Cause|Solution|
|:---------|:----|:-------|
|6713|Remote configuration services are unavailable. The remote configuration feature of the specified product is disabled.|Log on to the console, choose **Maintenance** \> **Remote Config**, and enable the remote configuration feature for the specified product.|
|6710|The system failed to query the remote configuration information.|Log on to the console, choose **Maintenance** \> **Remote Config**, and check whether you have edited the configuration file for the specified product.|

