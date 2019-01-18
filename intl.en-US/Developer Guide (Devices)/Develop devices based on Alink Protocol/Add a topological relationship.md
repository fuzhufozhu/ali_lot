# Add a topological relationship {#concept_cyv_ktw_y2b .concept}

After a sub-device has registered with IoT Platform, the gateway reports the topological relationship of [Gateways and sub-devices](../../../../../reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices/Gateways and sub-devices.md#) to IoT Platform before the sub-device connects to IoT Platform.

IoT Platform verifies the identity and the topological relationship during connection. If the verification is successful, IoT Platform establishes a logical connection with the sub-device and associates the logical connection with the physical connection of the gateway. The sub-device uses the same protocols as a directly connected device for data upload and download. Gateway information is not required to be included in the protocols.

After you delete the topological relationship of the sub-device from IoT Platform, the sub-device can no longer connect to IoT Platform through the gateway. IoT Platform will fail the authentication because the topological relationship does not exist.

## Add topological relationships of sub-devices {#section_w33_vyg_12b .section}

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add
-   Reply topic: sys/\{productKey\}/\{deviceName\}/thing/topo/add\_reply

Request data format when using the Alink protocol

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554",
      "sign": "xxxxxx",
      "signmethod": "hmacSha1",
      "timestamp": "1524448722000",
      "clientId": "xxxxxx"
    }
  ],
  "method": "thing.topo.add"
}
```

Response data format when using the Alink protocol

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String|Message ID. Reserve the parameter value for future use.|
|version|String |Protocol version. Currently, the value can only be 1.0.|
|params|List|Input parameters of the request.|
|deviceName|String |Device name. The value is the name of the sub-device.|
|productKey|String |Product ID. The value is the ID of the product to which the sub-device belongs.|
|sign|String |Signature.|
|signmethod|String |Signing method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String |Timestamp.|
|clientId|String |Identifier of a sub-device. This parameter is optional and may have the same value as ProductKey or DeviceName.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Signature algorithm

**Note:** IoT Platform supports common signature algorithms.

Sort all the parameters \(except for sign and signMethod\) that will be submitted to the server in lexicographical order, and then connect the parameters and values in turn \(no connect symbols \).

Sign the signing parameters by using the algorithm specified by the signing method.

For example, in the following request, sort the parameters in params in alphabetic order and then sign the parameters.

```
sign= hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp1524448722000)
```

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6402|topo relation cannot add by self|A device cannot be added to itself as a sub-device.|
|401|request auth error|Signature verification has failed.|

## Delete topological relationships of sub-devices {#section_rb1_wzw_y2b .section}

A gateway can publish a message to this topic to request IoT Platform to delete the topological relationship between the gateway and a sub-device.

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/delete\_reply

Request data format when using the Alink protocol

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ],
  "method": "thing.topo.delete"
}
```

Response data format when using the Alink protocol

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. Reserve the parameter value for future use.|
|version|String |Protocol version. Currently, the value can only be 1.0.|
|params|List|Request parameters.|
|deviceName|String |Device name. The value is the name of the sub-device.|
|productKey|String |Product ID. The value is the ID of the product to which the sub-device belongs.|
|method|String |Request method.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.|

## Obtain topological relationships of sub-devices {#section_zjz_xzw_y2b .section}

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/get
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/get\_reply

A gateway can publish a message to this topic to obtain the topological relationships between the gateway and its connected sub-devices.

Request data format when using the Alink protocol

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.topo.get"
}
```

Response data format when using the Alink protocol

```
{
  "id": "123",
  "code": 200,
  "data": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ]
}
```

​Parameter description​

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String |Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This can be left empty.|
|method|String |Request method.|
|deviceName|String |Name of the sub-device.|
|productKey|String |Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|

## Report new sub-devices {#section_sqt_yzw_y2b .section}

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/list/found
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/list/found\_reply

In some scenarios, the gateway can discover new sub-devices. The gateway reports information of a new sub-device to IoT Platform. IoT Platform forwards the sub-device information to third-party applications, and the third-party applications choose the sub-devices to connect to the gateway.

Request data format when using the Alink protocol

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ],
  "method": "thing.list.found"
}
```

Response data format when using the Alink protocol

```
{
  "id": "123",
  "code": 200,
  "data":{}
}
```

​Parameter description​

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String |Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This parameter can be left empty.|
|method|String |Request method.|
|deviceName|String |Name of the sub-device.|
|productKey|String |Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6250|product not found|The specified product to which the sub-device belongs does not exist.|
|6280|devicename not meet specs|The name of the sub-device is invalid. The device name must be 4 to 32 characters in length and can contain letters, digits, hyphens \(-\), underscores \(\_\), at signs \(@\), periods \(.\), and colons \(:\).|

## Notify the gateway to add topological relationships of the connected sub-devices {#section_cn4_zzw_y2b .section}

Downstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify\_reply

IoT Platform publishes a message to this topic to notify a gateway to add topological relationships of the connected sub-devices. You can use this topic together with the topic that reports new sub-devices to IoT Platform. IoT Platform can subscribe to a data exchange topic to receive the response from the gateway. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request data format when using the Alink protocol

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ],
  "method": "thing.topo.add.notify"
}
```

Response data format when using the Alink protocol

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String |Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This parameter can be left empty.|
|method|String |Request method.|
|deviceName|String |Name of the sub-device.|
|productKey|String |Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

