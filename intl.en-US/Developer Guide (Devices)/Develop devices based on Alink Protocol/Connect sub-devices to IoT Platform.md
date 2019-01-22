# Connect sub-devices to IoT Platform {#concept_zzf_mtw_y2b .concept}

Register devices with IoT Platform, relate the devices to a gateway device as sub-devices, and then connect these sub-devices to IoT Platform .

**Note:** A gateway device can have up to 1500 sub-devices.

Make sure that a sub-device has been registered with IoT Platform before connecting to IoT Platform. In addition, you also need to make sure that the topological relationship with the gateway has been added to the gateway. IoT Platform will verify the identity of the sub-device according to the topological relationship to identify whether the sub-device can use the gateway connection.

## Connect sub-devices to IoT Platform {#section_iqk_mzg_12b .section}

Upstream​

-   Topic: /ext/session/\{productKey\}/\{deviceName\}/combine/login
-   Reply topic: /ext/session/\{productKey\}/\{deviceName\}/combine/login\_reply

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

Response message

```
{
  "id":"123",
  "code":200,
  "message":"success"
  "data":""
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|params|Object|Request parameters.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ID of the product to which the sub-device belongs.|
|sign|String|Signature of a sub-device. Sub-devices use the same signature rules as the gateway.Sign algorithm:

Sort all the parameters \(except sign and signmethod\) to be submitted to the server in alphabetical order, and then splice the parameters and values in turn \(without splice symbols \).

Then, sign the parameters by using the algorithm specified by signMethod.

Example:

```
sign= hmac_md5(deviceSecret, cleanSessiontrueclientId123deviceNametestproductKey123timestamp123)
```

|
|signmethod|String|Sign method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String|Timestamp.|
|clientId|String|Identifier of a device client. This parameter can have the same value as the ProductKey or DeviceName parameter.|
|cleanSession|String|A value of true indicates that when the device is offline, messages sent based on QoS=1 method will be cleared.|
|code|Integer|Result code. A value of 200 indicates that the request is successful.|
|message|String|Result message.​|
|data|String|Additional information in the response, in JSON format.|

**Note:** A gateway can accommodate a maximum of 1500 concurrent online sub-devices. When the maximum number is reached, the gateway rejects any connection requests. 

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|429|rate limit, too many subDeviceOnline msg in one minute|The authentication requests from the device are throttled because the device requests authentication to IoT Platform too frequently.|
|428|too many subdevices under gateway|Too many sub-devices connect to the gateway at the same time.|
|6401|topo relation not exist|The topological relationship between the gateway and the sub-device does not exist.|
|6100|device not found|The sub-device does not exist.|
|521|device deleted|The sub-device has been deleted.|
|522|device forbidden|The sub-device has been disabled.|
|6287|invalid sign|The password or signature of the sub-device is incorrect.|

## Disconnect sub-devices from IoT Platform {#section_hkx_21x_y2b .section}

Upstream

-   Topic: /ext/session/\{productKey\}/\{deviceName\}/combine/logout
-   Reply topic: /ext/session/\{productKey\}/\{deviceName\}/combine/logout\_reply

Request message

```
{
  "id": 123,
  "params": {
    "productKey": "xxxxx",
    "deviceName": "xxxxx"
  }
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "message": "success",
  "data": ""
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the parameter for future use.|
|params|Object|Request parameters.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ID of the product to which the sub-device belongs.|
|code|Integer|Result code. A value of 200 indicates that the request is successful.|
|message|String|Result code.​|
|data|String|Additional information in the response, in JSON format.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|520|device no session|The sub-device session does not exist.|

For information about sub-device connections, see [Connect sub-devices to IoT Platform](../../../../../reseller.en-US/Developer Guide (Devices)/Connect sub-devices to the cloud/Connect sub-devices to IoT Platform.md#). For information about error codes, see [Error codes](../../../../../reseller.en-US/Developer Guide (Devices)/Error codes for sub-device development.md#).

