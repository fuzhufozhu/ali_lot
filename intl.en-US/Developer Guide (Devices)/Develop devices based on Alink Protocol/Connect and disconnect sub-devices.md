# Connect and disconnect sub-devices {#concept_zzf_mtw_y2b .concept}

Register devices with IoT Platform, assign the devices to a gateway device as sub-devices, and then connect these sub-devices to IoT Platform using the communication channel of the gateway device. When a sub-device is connecting to IoT Platform, IoT Platform verifies the identity of the sub-device according to the topological relationship between the gateway and the sub-device to identify whether the sub-device can use the channel of the gateway.

**Note:** For messages about sub-device connection and disconnection, the QoS is 0.

## Connect a sub-device to IoT Platform {#section_iqk_mzg_12b .section}

**Note:** A gateway device can have up to 1500 sub-devices connected to IoT Platform. When the maximum number is reached, IoT Platform will deny new connection requests from sub-devices of the gateway.

Upstream​

-   Request topic: `/ext/session/${productKey}/${deviceName}/combine/login`
-   Response topic: `/ext/session/${productKey}/${deviceName}/combine/login_reply`

**Note:** Because sub-devices use channels of gateways to communicate with IoT Platform, these topics are topics of gateway devices. Replace the variables $\{productKey\} and $\{deviceName\} in the topics with the corresponding information of the gateway device.

Request message

```
{
  "id": "123",
  "params": {
    "productKey": "123",
    "deviceName": "test",
    "clientId": "123",
    "timestamp": "123",
    "signMethod": "hmacmd5",
    "sign": "xxxxxx",
    "cleanSession": "true"
  }
}
```

**Note:** In the request message, the values of parameters productKey and deviceName are the corresponding information of the sub-device.

Response message:

```
{
  "id":"123",
  "code":200,
  "message":"success"
  "data":""
}
```

Request Parameters

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|params|Object|Request parameters.|
|deviceName|String|Name of the sub-device.|
|productKey|String|The unique identifier of the product to which the device belongs.|
|sign|String| Signature of the sub-device. Sub-devices use the same signature rules as gateways.

 Sign algorithm:

 1.  Sort all the parameters \(except sign and signMethod and cleanSession \) to be submitted to the server in alphabetical order, and then concatenate the parameters and values in turn \(without any delimiters\).
2.  Then, sign the parameters by using the algorithm specified by signMethod and the DeviceSecret of the sub-device.

 Example:

 ```
sign= hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp123)
```

 |
|signMethod|String|Sign method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String|Timestamp.|
|clientId|String|The device identifier. The value of this parameter can be the value of ProductKey and DeviceName.|
|cleanSession|String| -   A value of true indicates that when the sub-device is offline, messages sent based on QoS=1 method will be cleared.
-   A value of false indicates that when the sub-device is offline, messages sent based on QoS=1 method will not be cleared.

 |

Response parameters

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID.|
|code|Integer|Result code. A value of 200 indicates that the request is successful.|
|message|String|Result message.​|
|data|Object|Additional information in the response, in JSON format.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|429|rate limit, too many subDeviceOnline msg in one minute|The authentication requests from the device are limited because the device requested authentication to IoT Platform too frequently.|
|428|too many subdevices under gateway|The number of sub-devices connected to IoT Platform has reached the upper limit.|
|6401|topo relation not exist|The topological relationship between the gateway and the sub-device does not exist.|
|6100|device not found|The sub-device does not exist.|
|521|device deleted|The sub-device has been deleted.|
|522|device forbidden|The sub-device has been disabled.|
|6287|invalid sign|The password or signature of the sub-device is incorrect.|

## Disconnect a sub-device from IoT Platform {#section_hkx_21x_y2b .section}

Upstream

-   Request topic: `/ext/session/{productKey}/{deviceName}/combine/logout`
-   Response topic: `/ext/session/{productKey}/{deviceName}/combine/logout_reply`

**Note:** Because sub-devices use channels of gateways to communicate with IoT Platform, these topics are topics of gateway devices. Replace the variables $\{productKey\} and $\{deviceName\} in the topics with the corresponding information of the gateway device.

Request message:

```
{
  "id": 123,
  "params": {
    "productKey": "xxxxx",
    "deviceName": "xxxxx"
  }
}
```

**Note:** In the request message, the values of parameters productKey and deviceName are the corresponding information of the sub-device.

Response message:

```
{
  "id": "123",
  "code": 200,
  "message": "success",
  "data": ""
}
```

Request Parameters

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|params|Object|Request parameters.|
|deviceName|String|Name of the sub-device.|
|productKey|String|The unique identifier of the product to which the device belongs.|

Response parameters

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|The message ID.|
|code|Integer|Result code. A value of 200 indicates that the request is successful.|
|message|String|Result message.|
|data|Object|Additional information in the response, in JSON format.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|520|device no session|The sub-device session does not exist.|

For more information about sub-device connections, see [Device identity registration](reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Device identity registration.md#). For more information about error codes, see [Error codes](../../../../../reseller.en-US/Developer Guide (Devices)/Error codes for sub-device development.md#).

