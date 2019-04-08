# Error codes for sub-device development {#concept_nth_dgw_vdb .concept}

This article describes errors that may occur during sub-device development.

## Introduction {#section_ikc_fgw_vdb .section}

-   When an IoT Platform service error occurs on a directly-connected device, the user client is notified of the error when the TCP connection is closed.
-   In the case that a communication error occurs on a sub-device connected to IoT Platform through a gateway and the gateway is still physically connected to IoT Platform, the gateway must send an error message through the gateway connection to notify the user client of the error.

## Response format {#section_q2j_ggw_vdb .section}

When a communication error has occurred between a sub-device and IoT Platform, IoT Platform sends an MQTT error message to the gateway through the gateway connection.

The format of the topic varies depending on the scenario. The JSON format of the message content is as follows:

```

{
id: Message ID specified in the request parameters
code: Error code (the success code is 200)
message: Error message
}
```

## Sub-device failed to go online {#section_ylf_4gw_vdb .section}

The error message is sent to topic `/ext/session/{gw_productKey}/{gw_deviceName}/combine/login_reply`.

|Code|Message|Description|
|----|-------|-----------|
|460|request parameter error|Invalid parameter format, for example, invalid JSON format or invalid authentication parameters.|
|429|too many requests|Authentication requests have been denied. This error occurs when a device initiates authentication requests to IoT Platform too frequently or a sub-device has come online more than five times in one minute.|
|428|too many subdevices under gateway|The number of sub-devices connected to a gateway has reached the maximum. Currently, up to 1500 sub-devices can be connected to a gateway.|
|6401|topo relation not exist|No topological relationship has been established between the sub-device and the gateway.|
|6100|device not found|The specified sub-device does not exist.|
|521|device deleted|The sub-device has already been deleted.|
|522|device forbidden|The specified sub-device has been disabled.|
|6287|invalid sign|Authentication failed due to invalid username or password.|
|500|server error|An exception occurs on IoT Platform.|

## Sub-device automatically goes offline {#section_i4s_vhw_vdb .section}

The error message is sent to topic `/ext/session/{gw_productKey}/{gw_deviceName}/combine/logout_reply`.

|Code|Message|Description|
|----|-------|-----------|
|460|request parameter error|Invalid parameter format, for example, invalid JSON format or invalid parameters.|
|520|device no session|The sub-device session does not exist, because the sub-device has gone offline or has never been connected to IoT Platform..|
|500|server error|An exception occurs on IoT Platform.|

## Sub-device forced to go offline {#section_m3w_m3w_vdb .section}

The error message is sent to topic `/ext/error/{gw_productKey}/{gw_deviceName}`.

|Code|Message|Description|
|----|-------|-----------|
|427|device connect in elsewhere|Disconnection of current session. When another device uses the same device certificate of ProductKey, DeviceName, and DeviceSecret to connect to IoT Platform, the current device is forced offline.|
|521|device deleted|The device has been deleted.|
|522|device forbidden|The device has been disabled.|
|6401|topo relation not exist|The topological relationship between the sub-device and the gateway has been deleted.|

## Sub-device failed to send message {#section_uzw_bjw_vdb .section}

The error message is sent to topic /ext/error/\{gw\_productKey\}/\{gw\_deviceName\}.

|Code|Message|Description|
|----|-------|-----------|
|520|device session error|Sub-device session error.-   The sub-device session does not exist. The sub-device is not connected to IoT Platform or has gone offline.
-   The sub-device session exists, however, the session is not established through the current gateway.

|

