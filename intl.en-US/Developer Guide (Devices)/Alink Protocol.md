# Alink Protocol {#concept_lgm_24w_5db .concept}

IoT Platform provides device SDKs for you to configure devices. These device SDKs already encapsulate protocols for data exchange between devices and IoT Platform. However, in some cases, the device SDKs provided by IoT Platform cannot meet your requirements because of the complexity of the embedded system. This topic describes how to encapsulate data and establish connections from devices to IoT Platform using Alink protocol. Alink protocol is a data exchange standard for IoT development. Data are in JSON format.

## Connection process {#section_sp3_4vg_12b .section}

As shown in the following figure, devices can be connected to IoT Platform as directly connected devices or sub-devices. The connection process includes these key steps: register the device, establish a connection, and report data.

Directly connected devices can be connected to IoT Platform by using the following methods:

-   If [Unique-certificate-per-device authentication](../../../../reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-device authentication.md#) is enabled, install the three key fields \(ProductKey, DeviceName, and DeviceSecret\) into a device in advance, register the device with IoT Platform, connect the device to IoT Platform, and report data to IoT Platform.
-   If dynamic registration based on [Unique-certificate-per-product authentication](../../../../reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-product authentication.md#) is enabled, install the product certificate \(ProductKey and ProductSecret\) on a device, register the device with IoT Platform, connect the device to IoT Platform, and report data to IoT Platform.

The gateway starts the connection process for sub-devices. Sub-devices can be connected to IoT Platform by using the following methods:

-   If [Unique-certificate-per-device authentication](../../../../reseller.en-US/Developer Guide (Devices)/Authenticate devices /Unique-certificate-per-device authentication.md#) is enabled, install the ProductKey, DeviceName, and DeviceSecret on a sub-device. The sub-device sends these three key fields to the gateway. The gateway adds the topological relationship and sends the data of the sub-device through the gateway connection.
-   If dynamic registration is enabled, install ProductKey on a sub-device in advance. The sub-device sends the ProductKey and DeviceName to the gateway. The gateway forwards the ProductKey and DeviceName to IoT Platform. IoT Platform verifies the received DeviceName and sends a DeviceSecret to the sub-device. The sub-device sends the obtained ProductKey, DeviceName, and DeviceSecret to the gateway. The gateway adds the topological relationship and sends data to IoT Platform through the gateway connection.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7638/15350161555802_en-US.png)

## Device identity registration {#section_gm3_svg_12b .section}

The following methods are available for identity registration:

-   Unique certificate per device: Obtain the ProductKey, DeviceName, and DeviceSecret of a device on IoT Platform and use them as the unique identifier. Install these three key fields on the firmware of the device. After the device is connected to IoT Platform, the device starts to communicate with IoT Platform.
-   Dynamic registration: You can perform dynamic registration based on unique-certificate-per-product authentication for directly connected devices and perform dynamic registration for sub-devices.
    -   To dynamically register a directly connected device based on unique-certificate-per-product authentication, follow these steps:
        1.  In the IoT Platform console, pre-register the device and obtain the ProductKey and ProductSecret. When you pre-register the device, use device information that can be directly read from the device as the DeviceName, such as the MAC address or SN.
        2.  Enable dynamic registration in the console.
        3.  Install the product certificate on the device firmware.
        4.  The device authenticates to IoT Platform. If the device passes authentication, IoT Platform assigns a DeviceSecret to the device.
        5.  The device uses the ProductKey, DeviceName, and DeviceSecret to establish a connection to IoT Platform.
    -   To dynamically register a sub-device, follow these steps:
        1.  In the IoT Platform console, pre-register the sub-device and obtain the ProductKey. When you pre-register the sub-device, use device information that can be read directly from the sub-device as the DeviceName, such as the MAC address and SN.
        2.  Enable dynamic registration in the console.
        3.  Install the ProductKey on the firmware of the sub-device or on the gateway.
        4.  The gateway authenticates to IoT Platform on behalf of the sub-device.

**Dynamically register a sub-device**

Upstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/sub/register
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/sub/register\_reply

Request message

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
  "method": "thing.sub.register"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": [
    {
      "iotId": "12344",
      "productKey": "1234556554",
      "deviceName": "deviceName1234",
      "deviceSecret": "xxxxxx"
    }
  ]
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Parameters used for dynamic registration.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ProductKey of the sub-device.|
|iotId|String|Unique identifier of the sub-device.|
|deviceSecret|String|DeviceSecret key.|
|method|String|Request method.|
|code|Integer|Result code.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6402|topo relation cannot add by self|A device cannot be added to itself as a sub-device.|
|401|request auth error|Signature verification has failed.|

**Dynamically register a directly connected device based on unique-certificate-per-product authentication**

Directly connected devices send HTTP requests to perform dynamic register. Make sure that you have enabled dynamic registration based on unique certificate per product in the console.

-   URL template: https://iot-auth.cn-shanghai.aliyuncs.com/auth/register/device
-   HTTP method: POST

Request message

```
POST /auth/register/device HTTP/1.1
Host: iot-auth.cn-shanghai.aliyuncs.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 123
productKey=1234556554&deviceName=deviceName1234&random=567345&sign=adfv123hdfdh&signMethod=HmacMD5
```

Response message

```
{
  "code": 200,
  "data": {
    "productKey": "1234556554",
    "deviceName": "deviceName1234",
    "deviceSecret": "adsfweafdsf"
  },
  "message": "success"
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|productKey|String|Unique identifier of the product.|
|deviceName|String|Device name.|
|random|String|Random number.|
|sign|String|Signature.|
|signMethod|String|Signing method. The supported methods are hmacmd5, hmacsha1, and hmacsha256.|
|code|Integer|Result code.|
|deviceSecret|String|DeviceSecret key.|

Sign the parameters

All parameters reported to IoT Platform will be signed except sign and signMethod. Sort the signing parameters in alphabetical order,  and splice the parameters and values without any splicing symbols. Then, sign the parameters by using the algorithm specified by signMethod. `sign = hmac_sha1(productSecret, deviceNamedeviceName1234productKey1234556554random123)`

## Add topological relationships {#section_w33_vyg_12b .section}

After a sub-device has registered with IoT Platform, the gateway reports the topological relationship of [Gateways and sub-devices](../../../../reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices.md#) to IoT Platform before the sub-device connects to IoT Platform. IoT Platform verifies the identity and the topological relationship during connection. If the verification is successful, IoT Platform establishes a logical connection with the sub-device and associates the logical connection with the physical connection of the gateway. The sub-device uses the same protocols as a directly connected device for data upload and download. Gateway information is not required to be included in the protocols.

After you delete the topological relationship of the sub-device from IoT Platform, the sub-device can no longer connect to IoT Platform through the gateway. IoT Platform will fail the authentication because the topological relationship does not exist.

**Add topological relationships of sub-devices**

Upstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add\_reply

Request message

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

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Request parameters.|
|deviceName|String|Device name. The value is the name of a sub-device.|
|productKey|String|ProductKey. The value is the name of a sub-device.|
|sign|String|Signature.|
|signmethod|String|Signing method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String|Timestamp.|
|clientId|String|Identifier of a sub-device. This parameter is optional and may have the same value as ProductKey or DeviceName.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Signature algorithm

**Note:** IoT Platform supports common signature algorithms.

1.  All parameters reported to IoT Platform will be signed except sign and signMethod. Sort the signing parameters in alphabetical order,  and splice the parameters and values without any splicing symbols. Sign the signing parameters by using the algorithm specified by the signing method.
2.  For example, sign the parameters in params in the request as follows:
3.  `sign=hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp1524448722000)`

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6402|topo relation cannot add by self|A device cannot be added to itself as a sub-device.|
|401|request auth error|Signature verification has failed.|

**Delete topological relationships of sub-devices**

A gateway can publish a message to this topic to request IoT Platform to delete the topological relationship between the gateway and a sub-device.

Upstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/delete\_reply

Request message

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

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Request parameters.|
|deviceName|String|DeviceName. This is the name of a sub-device.|
|productKey|String|ProductKey. This is the ProductKey of a sub-device.|
|method|String|Request method.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6100|device not found|The device does not exist.|

**Obtain topological relationships of sub-devices**

Upstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/get
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/get\_reply

A gateway can publish a message to this topic to obtain the topological relationships between the gateway and its connected sub-devices.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.topo.get"
}
```

Response message

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

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This can be left empty.|
|method|String|Request method.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ProductKey of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|

**Report new sub-devices**

Upstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/list/found
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/list/found\_reply. In some scenarios, the gateway can discover new sub-devices. The gateway reports information of a sub-device to IoT Platform. IoT Platform forwards sub-device information to third-party applications, and the third-party applications choose the sub-devices to connect to the gateway.

Request message

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

Response message

```
{
  "id": "123",
  "code": 200,
  "data":{}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This can be left empty.|
|method|String|Request method.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ProductKey of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6250|product not found|The sub-device does not exist.|
|6280|devicename not meet specs|The name of the sub-device is invalid. The device name must be 4 to 32 characters in length and can contain letters, numbers, hyphens \(-\), underscores \(\_\), at signs \(@\), periods \(.\), and colons \(:\).|

**Notify the gateway to add topological relationships of the connected sub-devices**

Downstream

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify\_reply

IoT Platform publishes a message to this topic to notify a gateway to add topological relationships of the connected sub-devices. You can use this topic together with the topic that reports new sub-devices to IoT Platform. IoT Platform can subscribe to a data exchange topic to receive the response from the gateway. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request message

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

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This can be left empty.|
|method|String|Request method.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ProductKey of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

## Connect devices to IoT Platform {#section_iqk_mzg_12b .section}

Make sure that a directly connected device has been registered with IoT Platform before connecting to IoT Platform.

Make sure that a sub-device has been registered with IoT Platform before connecting to IoT Platform. In addition, you also need to make sure that the topological relationship with the gateway has been added to the gateway. IoT Platform will verify the identity of the sub-device according to the topological relationship to identify whether the sub-device can use the gateway connection.

**Connect sub-devices to IoT Platform**

Upstream

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

Sign the parameters

1.  All parameters reported to IoT Platform will be signed except sign and signMethod. Sort the signing parameters in alphabetical order,  and splice the parameters and values without any splicing symbols. Then, sign the parameters by using the algorithm specified by signMethod.
2.  `sign= hmac_md5(deviceSecret, cleanSessiontrueclientId123deviceNametestproductKey123timestamp123)`

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|params|List|Input parameters of the request.|
|deviceName|String|DeviceName of a sub-device.|
|productKey|String|ProductKey of a sub-device.|
|sign|String|Signature of a sub-device. Sub-devices use the same signature rules as the gateway.|
|signmethod|String|Signing method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String|Timestamp.|
|clientId|String|Identifier of a device client. This parameter can have the same value as the ProductKey or DeviceName parameter.|
|cleanSession|String|If the value is true, this indicates to clear offline information for all sub-devices, which is information that has not been received by QoS 1.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|
|message|String|Result code.|
|data|String|Additional information in the response, in JSON format.|

**Note:** A gateway can accommodate a maximum of 200 concurrent online sub-devices. When the maximum number is reached, the gateway rejects any connection requests. 

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|429|rate limit, too many subDeviceOnline msg in one minute|The authentication requests from the device are throttled because the device requests authentication to IoT Platform too frequently.|
|428|too many subdevices under gateway|Too many sub-devices connect to the gateway at the same time.|
|6401|topo relation not exist|The topological relationship between the gateway and the sub-device does not exist.|
|6100|device not found|The sub-device does not exist.|
|521|device deleted|The sub-device has been deleted.|
|522|device forbidden|The sub-device has been disabled.|
|6287|invalid sign|The password or signature of the sub-device is incorrect.|

**Disconnect sub-devices from IoT Platform**

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

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|params|List|Input parameters of the request.|
|deviceName|String|DeviceName of a sub-device.|
|productKey|String|ProductKey of a sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|
|message|String|Result code.|
|data|String|Additional information in the response, in JSON format.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|520|device no session|The sub-device session does not exist.|

For information about sub-device connections,  see [Connect sub-devices to IoT Platform](../../../../reseller.en-US/Developer Guide (Devices)/Connect sub-devices to the cloud/Connect sub-devices to IoT Platform.md#). For information about error codes, see [Error codes](../../../../reseller.en-US/Developer Guide (Devices)/Connect sub-devices to the cloud/Error codes.md#).

## Device property, event, and service protocols {#section_g4j_5zg_12b .section}

A device sends data to IoT Platform either in standard mode or in passthrough mode.

1.  If passthrough mode is used, the device sends raw data such as a binary data stream to IoT Platform. IoT Platform runs the script you have submitted to convert the raw data to a standard format.  
2.  If standard mode is used, the device generates data in the standard format and then sends the data to IoT Platform. For information about standard formats, see the requests and responses in this topic.

**Report device properties**

**Note:** Set the parameters according to the output and input parameters in the TSL model.

Upstream \(passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Upstream \(non-passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/event/property/post
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "Power": {
      "value": "on",
      "time": 1524448722000
    },
    "WF": {
      "value": 23.6,
      "time": 1524448722000
    }
  },
  "method": "thing.event.property.post"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters.|
|method|String|Request method.|
|Power|String|Property name.|
|value|String|Property value.|
|time|Long|UTC timestamp in milliseconds.|
|code|Integer|Result code.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.|
|6106|map size must less than 200|A device can report a maximum of 200 properties at any one time.|
|6313|tsl service not available|When a device reports a property to IoT Platform, IoT Platform examines whether the property format is the same as the predefined format. This error message occurs when this verification service is unavailable.  For more information about property verification, see [Define Thing Specification Language models](../../../../reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#)|

**Note:** IoT Platform compares the format of each reported property with the predefined format in the TSL model to verify the validity of the property. IoT Platform directly drops invalid properties and keeps only the valid properties. If all properties are invalid, IoT Platform drops all properties. However, the response returned to the device will still indicate that the request is successful.

**Set device properties**

**Note:** Set the parameters according to the output and input parameters in the TSL model.

Downstream \(passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply

Downstream \(non-passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/set
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/set\_reply

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "temperature": "30.5"
  },
  "method": "thing.service.property.set"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Parameters that are used to set the properties.|
|method|String|Request method.|
|temperature|String|Property name.|
|code|Integer|Result code. For more information, see the common codes on the device.|

**Get device properties**

**Note:** 

-   Set the parameters according to the output and input parameters in the TSL model.
-   Currently, device properties are retrieved from the device shadow.

Downstream \(passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply

After a device receives a request to get the device properties, it returns the obtained properties in a reply message to IoT Platform. IoT Platform can subscribe to a data exchange topic to obtain the returned properties. The data exchange topic is `{productKey}/{deviceName}/thing/downlink/reply/message`.

payload: 0x001FFEE23333

Downstream \(non-passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/get
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/service/property/get\_reply

After a device receives a request to get the device properties, it returns the obtained properties in a reply message. IoT Platform subscribes to a data exchange topic to obtain the returned properties. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    "power",
    "temp"
  ],
  "method": "thing.service.property.get"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {
    "power": "on",
    "temp": "23"
  }
}
```

Parameter description

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Names of the desired properties.|
|method|String|Request method.|
|power|String|Property name.|
|temp|String|Property name.|
|code|Integer|​Result code.​|

**​Report device events​**

Upstream \(passthrough\)​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/model/up\_raw\_reply

Upstream \(non-passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post\_reply

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "value": {
      "Power": "on",
      "WF": "2"
    },
    "time": 1524448722000
  },
  "method": "thing.event.{tsl.event.identifier}.post"
}
```

Response message

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Parameters of the event to be reported.​|
|method|String|Request method.|
|value|Object|Parameter values of the event.​|
|Power|String|Parameter name of the event. You can select a parameter name based on the TSL model.​|
|WF|String|Parameter name of the event. You can select a parameter name based on the TSL model.​|
|code|Integer|Result code. For more information, see the common codes on the device.​|
|time|Long|UTC timestamp in milliseconds.​|

**Note:** 

-   tsl.event.identifier is the identifier of the event in the TSL model. For information about TSL models, see [Define Thing Specification Language models](../../../../reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#).
-   IoT Platform will compare the format of the received event with the event format that was predefined in the TSL model to verify the validity of the event. IoT Platform directly drops an invalid event and returns a message indicating that the request has failed.

**Invoke device services**

Downstream \(passthrough\)

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw
-   Rely topic: /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply

If the invoke method of a service is set to Synchronous in the console, IoT Platform uses RRPC to publish a message synchronously to this topic.

If the invoke method of a service is set to Asynchronous in the console, IoT Platform publishes a message asynchronously to this topic. The device also replies asynchronously. IoT Platform subscribes to the asynchronous reply topic only after the invoke method of the current service has been set to Asynchronous. This reply topic is /sys/\{productKey\}/\{deviceName\}/thing/model/down\_raw\_reply. IoT Platform subscribes to a data exchange topic to obtain the result for an asynchronous call. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Downstream \(non-passthrough\)

-   ​Topic: /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\}
-   ​Reply Topic: /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\}\_reply

If the invoke method of a service is set to Synchronous in the console, IoT Platform uses RRPC to publish a message synchronously to this topic.

If the invoke method of a service is set to Asynchronous in the console, IoT Platform publishes a message asynchronously to this topic. The device also replies asynchronously.  IoT Platform subscribes to the asynchronous reply topic only after the invoke method of the current service has been set to Asynchronous. The reply topic is /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\}\_reply. IoT Platform subscribes to a data exchange topic to obtain the result for an asynchronous call. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "Power": "on",
    "WF": "2"
  },
  "method": "thing.service.{tsl.service.identifier}"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Parameters of the service to be invoked.​|
|method|String|Request method.|
|value|Object|Parameter name of the service.​|
|Power|String|Parameter name of the service.​|
|WF|String|Parameter name of the service.​|
|code|Integer|Result code. For more information, see the common codes on the device.|

**Note:** tsl.service.identifier is the identifier of the service that has been defined in the TSL model. For more information about TSL models, see [Define Thing Specification Language models](../../../../reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#).

## Disable and delete devices {#section_xwd_p1h_12b .section}

**Disable devices​**

Downstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/disable
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/disable\_reply

This topic disables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic.  Gateways can subscribe to this topic to disable the corresponding sub-devices.

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.disable"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Result code. For more information, see the common codes on the device.|

**Enable devices​**

Downstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/enable
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/enable\_reply

This topic enables a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to enable the corresponding sub-devices.

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.enable"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|Integer|Result code. For more information, see the common codes on the device.|

**Delete devices**

Downstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/delete\_reply

This topic deletes a device connection. IoT Platform publishes messages to this topic asynchronously, and the devices subscribe to this topic. Gateways can subscribe to this topic to delete the corresponding sub-devices.

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.delete"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|​Type​|Description|
|:---------|:-----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. Leave empty.|
|method|String|Request method.|
|code|String|Result code. For more information, see the common codes on the device.|

## Device tags {#section_hnn_z1h_12b .section}

**Report tags**

Upstrea​m​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/update
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/update\_reply

Device information such as vendor and device model, and static extended information can be saved as device tags.

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "attrKey": "Temperature",
      "attrValue": "36.8"
    }
  ],
  "method": "thing.deviceinfo.update"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object| Request parameters.

 This parameter can contain a maximum of 200 items.​

 |
|method|String|Request method.|
|attrKey|String| Tag name.

 -   ​Length: Up to 100 bytes.​
-   Valid characters: Lowercase letters a to z, uppercase letters A to Z, numbers 0 to 9, and underscores \(\_\).
-   The tag name must start with an English letter or underscore \(\_\).

 |
|attrValue|String|Tag value.​|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error code

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.​|

**Delete tags​**

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/delete
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/deviceinfo/delete\_reply

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "attrKey": "Temperature"
    }
  ],
  "method": "thing.deviceinfo.delete"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters.|
|method|String|Request method.|
|attrKey|String| Tag name.​

 -   Length: Up to 100 bytes.
-   Valid characters: Lowercase letters a to z, uppercase letters A to Z, numbers 0 to 9, and underscores \(\_\).
-   The tag name must start with an English letter or underscore \(\_\).

 |
|attrValue|String|Tag value.​|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.​|

## TSL models {#section_fr1_fbh_12b .section}

A device can publish requests to this topic to obtain the [Device TSL model](../../../../reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#) from IoT Platform.

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/dsltemplate/get
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/dsltemplate/get\_reply

Request message​

```
{
  "id": "123",
  "version": "1.0",
  "params": {},
  "method": "thing.dsltemplate.get"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {
    "schema": "https://iot-tsl.oss-cn-shanghai.aliyuncs.com/schema.json",
    "link": "/sys/1234556554/airCondition/thing/",
    "profile": {
      "productKey": "1234556554",
      "deviceName": "airCondition"
    },
    "properties": [
      {
        "identifier": "fan_array_property",
        "name": "Fan array property",
        "accessMode": "r",
        "required": true,
        "dataType": {
          "type": "array",
          "specs": {
            "size": "128",
            "item": {
              "type": "int"
            }
          }
        }
      }
    ],
    "events": [
      {
        "identifier": "alarm",
        "name": "alarm",
        "desc": "Fan alert",
        "type": "alert",
        "required": true,
        "outputData": [
          {
            "identifier": "errorCode",
             "name": "Error code",
            "dataType": {
              "type": "text",
              "specs": {
                "length": "255"
              }
            }
          }
        ],
        "method": "thing.event.alarm.post"
      }
    ],
    "services": [
      {
        "identifier": "timeReset", 
        "name": "timeReset",
        "desc": "Time calibration",
        "inputData": [
          {
            "identifier": "timeZone",
            "name": "Time zone",
            "dataType": {
              "type": "text",
              "specs": {
                "length": "512"
              }
            }
          }
        ],
        "outputData": [
          {
            "identifier": "curTime",
            "name": "Current time",
            "dataType": {
              "type": "date",
              "specs": {}
            }
          }
        ],
        "method": "thing.service.timeReset"
      },
      {
        "identifier": "set",
        "name": "set",
        "required": true,
        "desc": "Set properties",
        "method": "thing.service.property.set",
        "inputData": [
          {
            "identifier": "fan_int_property",
            "name": "Integer property of the fan",
            "accessMode": "rw",
            "required": true,
            "dataType": {
              "type": "int",
              "specs": {
                "min": "0",
                "max": "100",
                "unit": "g/ml",
                "unitName": "Millilitter"
              }
            }
          }
        ],
        "outputData": []
      },
      {
        "identifier": "get",
        "name": "get",
        "required": true,
        "desc": "Get properties",
        "method": "thing.service.property.get",
        "inputData": [
          "array_property",
          "fan_int_property",
          "batch_enum_attr_id",
          "fan_float_property",
           "fan_double_property",
          "fan_text_property",
          "Maid ",
          "batch_boolean_attr_id",
          "fan_struct_property"
        ],
        "outputData": [
          {
            "identifier": "fan_array_property",
            "name": "Fan array property",
            "accessMode": "r",
            "required": true,
            "dataType": {
              "type": "array",
              "specs": {
                "size": "128",
                "item": {
                  "type": "int"
                }
              }
            }
          }
        ]
      }
    ]
  }
}
```

Parameter description

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Leave empty.​|
|method|String|Request method.|
|productKey|String|ProductKey. This is 1234556554 in this example.|
|deviceName|String|Device name. This is airCondition in this example.​|
|data|Object|TSL model of the device. For more information, see[Define Thing Specification Language models](../../../../reseller.en-US/User Guide/Create products and devices/IoT Platform Pro/Define Thing Specification Language models.md#)|

Error code

|Error code|Message|Description|
|:---------|:------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6321|tsl: device not exist in product|The device does not exist.​|

## Update firmware {#section_ivm_fbh_12b .section}

For information about the firmware update, see [Develop OTA features](../../../../reseller.en-US//Develop OTA features.md#) and [Firmware update](../../../../reseller.en-US/User Guide/Extended services/Firmware update.md#).

**Report the firmware version**

Upstream​

-   Topic: /ota/device/inform/\{productKey\}/\{deviceName\}. The device publishes a message to this topic to report the current firmware version to IoT Platform.

​Request message​

```
{
  "id": 1,
  "params": {
    "version": "1.0.1"
  }
}
```

Parameter description​

|Parameters|​Type​|Description|
|:---------|:-----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Version information of the firmware.|

**Push firmware information​**

Upstream​

-   Topic: /ota/device/upgrade/\{productKey\}/\{deviceName\}

IoT Platform publishes messages to this topic to push firmware information. The devices subscribe to this topic to obtain the firmware information.

​Request message

```
{
  "code": "1000",
  "data": {
    "size": 432945,
    "version": "2.0.0",
    "url": "https://iotx-ota-pre.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
    "md5": "93230c3bde425a9d7984a594ac55ea1e",
    "sign": "93230c3bde425a9d7984a594ac55ea1e",
    "signMethod": "Md5"
  },
  "id": 1507707025,
  "message": "success"
}
```

​Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|message|String|Result code.​|
|version|String|Version information of the firmware.|
|size|Long|Firmware size in bytes.|
|url|String|OSS address of the firmware.|
|sign|String|​Firmware signature.​|
|signMethod|String|Signing method. Currently, the supported methods are MD5 and sha275.|
|md5|String|This parameter is reserved. This parameter is used to be compatible with old device information. When the signing method is MD5, IoT Platform will assign values to both the sign and md5 parameters.|

**Report update progress**

Upstream​

-   Topic: /ota/device/progress/\{productKey\}/\{deviceName\}

A device subscribes to this topic to report the firmware update progress.

Request message​

```
{
  "id": 1,
  "params": {
    "step": "-1",
    "desc": "Firmware update has failed. No firmware information is available."
  }
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|step|String| Firmware upgrade progress information.

 Values:

-   A value from 1 to 100 indicates the progress percentage.
-   A value of -1 indicates the firmware update has failed.
-   A value of -2 indicates that the firmware download has failed.
-   A value of -3 indicates that firmware verification has failed.
-   A value of -4 indicates that the firmware installation has failed.

 |
|desc|String|Description of the current step. If an exception occurs, this parameter displays an error message.|

**Request firmware information from IoT Platform**

-   Topic: /ota/device/request/\{productKey\}/\{deviceName\}

Request message​

```
{
  "id": 1,
  "params": {
    "version": "1.0.1"
  }
}
```

Parameter description

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Version information of the firmware.|

## Remote configuration {#section_xg4_fbh_12b .section}

**Request configuration information from IoT Platform**

Upstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/config/get
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/config/get\_reply

Request message​

```
{
  "id": 123,
  "version": "1.0",
  "params": {
    "configScope": "product",
    "getType": "file"
  },
  "method": "thing.config.get"
}
```

Response message

```
{
  "id": "123",
  "version": "1.0",
  "code": 200,
  "data": {
    "configId": "123dagdah",
    "configSize": 1234565,
    "sign": "123214adfadgadg",
    "signMethod": "Sha256",
    "url": "https://iotx-config.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
    "getType": "file"
  }
}
```

Parameter description​

|Parameters|Type|Description|
|:---------|:---|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|configScope|String|Configuration scope. Currently, IoT Platform supports only product dimension configuration. Set the value to product.|
|getType|String|Type of the desired configuration. Currently, the supported type is file. Set the value to file.|
|configId|String|Configuration ID.​|
|configSize|Long|Configuration size in bytes​.|
|sign|String|Signature.|
|signMethod|String|Signing method. The supported signing method is Sha256.​|
|url|String|OSS address of the confguration.​|
|code|Integer|Result code. A value of 200 indicates that the request is successful.|

Error code

|Error code|Message|Description|
|:---------|:------|:----------|
|6713|thing config function is not available|Remote configuration is disabled for the device. Enable remote configuration for the device.|
|6710|no data|No configuration data is available.​|

**Push configurations​**

Downstream​

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/config/push
-   Reply topic: /sys/\{productKey\}/\{deviceName\}/thing/config/push\_reply

A device subscribes to this topic to obtain configurations that have been pushed by IoT Platform. After you have configured configuration push for multiple devices in the IoT Platform console, IoT Platform pushes configurations to the devices asynchronously. IoT Platform subscribes to a data exchange topic to obtain the result that is returned by the device. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request message

```
{
  "id": "123",
  "version": "1.0",
  "params": {
    "configId": "123dagdah",
    "configSize": 1234565,
    "sign": "123214adfadgadg",
    "signMethod": "Sha256",
    "url": "https://iotx-config.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
    "getType": "file"
  },
  "method": "thing.config.push"
}
```

Response message​

```
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

Parameter description​

|Parameters|Type​|Description|
|:---------|:----|:----------|
|id|Long|Message ID. Reserved parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|configScope|String|Configuration scope. Currently, IoT Platform supports only product dimension configuration. Set the value to product.|
|getType|String|Type of the desired configuration. The supported type is files. Set the value to file.|
|configId|String|Configuration ID.​|
|configSize|Long|Configuration size in bytes.|
|sign|String|Signature.|
|signMethod|String|Signing method. The supported signing method is Sha256.|
|url|String|OSS address of the confguration.​|
|code|Integer|Result code. For more information, see the common codes on the device.|

## Common codes on devices {#section_bqp_fbh_12b .section}

Common codes on devices indicate the results that are returned to IoT Platform in response to requests from IoT Platform.

|Result code|Message|Description|
|:----------|:------|:----------|
|200|success|The request is successful.|
|400|request error|Internal service error.|
|460|request parameter error|The request parameters are invalid. The device has failed input parameter verification.|
|429|too many requests|The system is busy. This code can be used when the device is too busy to process the request.|
|100000-110000| |Devices use numbers from 100000 to 110000 to indicate device-specific error messages to distinguish them from error message on IoT Platform.​|

