# Device identity registration {#concept_gm3_jtw_y2b .concept}

Before you connect a device to IoT Platform, you need to register the device identity to identify it on IoT Platform.

The following methods are available for identity registration:

-   Unique certificate per device: Obtain the ProductKey, DeviceName, and DeviceSecret of a device on IoT Platform and use them as the unique identifier. Install these three key fields on the firmware of the device. After the device is connected to IoT Platform, the device starts to communicate with IoT Platform.
-   Dynamic registration: You can perform dynamic registration based on unique-certificate-per-product authentication for directly connected devices and perform dynamic registration for sub-devices.
    -   To dynamically register a directly connected device based on unique-certificate-per-product authentication, follow these steps:
        1.  In the IoT Platform console, pre-register the device and obtain the ProductKey and ProductSecret. When you pre-register the device, use device information that can be directly read from the device as the DeviceName, such as the MAC address or the serial number of the device.
        2.  Enable dynamic registration in the console.
        3.  Install the product certificate on the device firmware.
        4.  The device authenticates to IoT Platform. If the device passes authentication, IoT Platform assigns a DeviceSecret to the device.
        5.  The device uses the ProductKey, DeviceName, and DeviceSecret to establish a connection to IoT Platform.
    -   To dynamically register a sub-device, follow these steps:
        1.  In the IoT Platform console, pre-register a sub-device and obtain the ProductKey. When you pre-register the sub-device, use device information that can be read directly from the sub-device as the DeviceName, such as the MAC address and SN.
        2.  Enable dynamic registration in the console.
        3.  Install the ProductKey on the firmware of the sub-device or on the gateway.
        4.  The gateway authenticates to IoT Platform on behalf of the sub-device.

## Dynamically register a sub-device {#section_xfq_zww_y2b .section}

Upstream​

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

​Parameter description​

|Parameter|Type |Description|
|:--------|:----|:----------|
|id|String|Message ID. Reserve the value of the parameter for future use.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Parameters used for dynamic registration.|
|deviceName|String|Name of the sub-device.|
|productKey|String|ID of the product to which the sub-device belongs.|
|iotId|String|Unique identifier of the sub-device.|
|deviceSecret|String|DeviceSecret key.|
|method|String|Request method.|
|code|Integer|Result code.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6402|topo relation cannot add by self|A device cannot be added to itself as a sub-device.|
|401|request auth error|Signature verification has failed.|

## Dynamically register a directly connected device based on unique-certificate-per-product authentication {#section_efq_cxw_y2b .section}

Directly connected devices send HTTP requests to perform dynamic register. Make sure that you have enabled dynamic registration based on unique certificate per product in the console.

-   URL template: `https://iot-auth.cn-shanghai.aliyuncs.com/auth/register/device`
-   HTTP method： POST

Request message

```
POST /auth/register/device  HTTP/1.1
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

|Parameter|Type |Description|
|:--------|:----|:----------|
|productKey|String|ID of the product to which the device belongs.|
|deviceName|String|Name of the device|
|random|String|Random number.|
|sign|String|Signature.|
|signMethod|String|Signing method. The supported methods are hmacmd5, hmacsha1, and hmacsha256.|
|code|Integer|​Result code.​|
|deviceSecret|String|DeviceSecret key.|

Sign the parameters

All parameters reported to IoT Platform will be signed except sign and signMethod. Sort the signing parameters in alphabetical order, and splice the parameters and values without any splicing symbols.

Then, sign the parameters by using the algorithm specified by signMethod.

Example:

```
sign = hmac_sha1(productSecret, deviceNamedeviceName1234productKey1234556554random123)
```

