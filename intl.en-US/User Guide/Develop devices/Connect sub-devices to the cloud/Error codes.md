# Error codes {#concept_nth_dgw_vdb .concept}

## Overview {#section_ikc_fgw_vdb .section}

-   When an IoT Platform service error occurs for a directly-connected device, the user client is notified of the error through the TCP disconnection.
-   When a communication error occurs on a sub-device connected to IoT Platform through a gateway, the gateway is still physically connected to IoT Platform. In this case, the gateway must send an error message through the gateway connection to notify the user client of the error.

## Response format {#section_q2j_ggw_vdb .section}

When a communication error has occurred between a sub-device and IoT Platform, IoT Platform sends an MQTT error message to the gateway through the gateway connection.

The format of the topic varies depending on the scenario. The JSON format of the message content is as follows:

```

{
id: ID of a sub-device specified in request parameters.
code: Error code (the success code is 200)
message: Error message.
}
```

## Sub-device failed to go online {#section_ylf_4gw_vdb .section}

The error message is sent to topic /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/login\_reply.

|Code|Message|Remarks|
|----|-------|-------|
|460|request parameter error|Invalid parameter format, for example, invalid JSON format or invalid authentication parameters.|
|429|too many requests|Authentication requests have been denied. This error occurs when a device authenticates to IoT Platform too frequently or a sub-device has come online more than five times in one minute.|
|428|too many subdevices under gateway|The number of sub-devices connected to a gateway has reached the maximum. Currently, up to 200 sub-devices can be connected to a gateway.|
|6401|topo relation not exist|No topological relationship has been established between the sub-device and gateway.|
|6100|device not found|The specified sub-device does not exist.|
|521|device deleted|The sub-device has already been deleted.|
|522|device forbidden|The specified sub-device has been disabled.|
|6287|invalid sign|Authentication failed due to invalid username or password.|
|500|server error|IoT Platform error.|

## Sub-device automatically goes offline {#section_i4s_vhw_vdb .section}

The error message is sent to topic /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/logout\_reply.

|Code|Message|Remarks|
|----|-------|-------|
|460|request parameter error|Invalid parameter format, JSON format, or parameters.|
|520|device no session|The sub-device session does not exist. The sub-device has gone offline or the sub-device has never come online.|
|500|server error|IoT Platform error.|

## Sub-device forced to go offline {#section_m3w_m3w_vdb .section}

The error message is sent to topic /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/logout\_reply.

|Code|Message|Remarks|
|----|-------|-------|
|427|device connect in elsewhere|Disconnection of current session. When another device with the same set of ProductKey, DeviceName, and DeviceSecret comes online, the current device is forced offline.|
|521|device deleted|The device has been deleted.|
|522|device forbidden|The device has been disabled.|

## Sub-device failed to send message {#section_uzw_bjw_vdb .section}

The error message is sent to topic /ext/error/\{gw\_productKey\}/\{gw\_deviceName\}.

|Code|Message|Remarks|
|----|-------|-------|
|520|device session error|Sub-device session error.-   The sub-device session does not exist. The sub-device is not connected to IoT Platform or has gone offline.
-   The sub-device session exists. However, the session is not established through the current gateway.

|

