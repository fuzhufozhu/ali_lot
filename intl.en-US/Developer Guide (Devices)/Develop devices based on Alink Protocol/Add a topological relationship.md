# Add a topological relationship {#concept_cyv_ktw_y2b .concept}

After a sub-device has registered with IoT Platform, the gateway reports the topological relationship of gateways and sub-devices to IoT Platform before the sub-device connects to IoT Platform.

IoT Platform verifies the identity and the topological relationship during connection. If the verification is successful, IoT Platform establishes a logical connection with the sub-device and associates the logical connection with the physical connection of the gateway. The sub-device uses the same protocols as a directly connected device for data upload and download. Gateway information is not required to be included in the protocols.

After you delete the topological relationship of the sub-device from IoT Platform, the sub-device can no longer connect to IoT Platform through the gateway. IoT Platform will fail the authentication because the topological relationship does not exist.

## Add topological relationships of sub-devices {#section_w33_vyg_12b .section}

Upstream​

-   Request topic: `/sys/{productKey}/{deviceName}/thing/topo/add`
-   Reply topic: `sys/{productKey}/{deviceName}/thing/topo/add_reply`

Request data format when using the Alink protocol

``` {#codeblock_duf_sa5_k7g}
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
  ]
}
```

Response data format when using the Alink protocol

``` {#codeblock_kvr_gcy_tbv}
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Input parameters of the request.|
|deviceName|String|Device name. The value is the name of the sub-device.|
|productKey|String|Product ID. The value is the ID of the product to which the sub-device belongs.|
|sign|String|Signature. Signature algorithm:

 Sort all the parameters \(except for sign and signMethod\) that will be submitted to the server in lexicographical order, and then connect the parameters and values in turn \(no connect symbols \).

 Sign the signing parameters by using the algorithm specified by the signing method.

 For example, in the following request, sort the parameters in params in alphabetic order and then sign the parameters.

``` {#codeblock_mgs_wz6_k9z}
sign= hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp1524448722000)
```

 |
|signmethod|String|Signing method. The supported methods are hmacSha1, hmacSha256, hmacMd5, and Sha256.|
|timestamp|String|Timestamp.|
|clientId|String|Identifier of a sub-device. This parameter is optional and may have the same value as ProductKey or DeviceName.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6402|topo relation cannot add by self|A device cannot be added to itself as a sub-device.|
|401|request auth error|Signature verification has failed.|

## Delete topological relationships of sub-devices {#section_rb1_wzw_y2b .section}

A gateway can publish a message to this topic to request IoT Platform to delete the topological relationship between the gateway and a sub-device.

Upstream​

-   Request topic: `/sys/{productKey}/{deviceName}/thing/topo/delete`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/topo/delete_reply`

Request data format when using the Alink protocol

``` {#codeblock_33n_kgr_0hl}
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ]
}
```

Response data format when using the Alink protocol

``` {#codeblock_wsn_txd_4wj}
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|List|Request parameters.|
|deviceName|String|Device name. The value is the name of the sub-device.|
|productKey|String|Product ID. The value is the ID of the product to which the sub-device belongs.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6100|device not found|The device does not exist.|

## Obtain topological relationships of sub-devices {#section_zjz_xzw_y2b .section}

Upstream​

-   Request topic: `/sys/{productKey}/{deviceName}/thing/topo/get`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/topo/get_reply`

A gateway can publish a message to this topic to obtain the topological relationships between the gateway and its connected sub-devices.

Request data format when using the Alink protocol

``` {#codeblock_z1h_0cn_7sm}
{
  "id": "123",
  "version": "1.0",
  "params": {}
}
```

Response data format when using the Alink protocol

``` {#codeblock_3t1_06h_jhs}
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

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This can be left empty.|
|deviceName|String|Name of the sub-device.|
|productKey|String|Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|

## Report new sub-devices {#section_sqt_yzw_y2b .section}

Upstream​

-   Request topic: `/sys/{productKey}/{deviceName}/thing/list/found`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/list/found_reply`

In some scenarios, the gateway can discover new sub-devices. The gateway reports information of a new sub-device to IoT Platform. IoT Platform forwards the sub-device information to third-party applications, and the third-party applications choose the sub-devices to connect to the gateway.

Request data format when using the Alink protocol

``` {#codeblock_sgj_0ne_tdw}
{
  "id": "123",
  "version": "1.0",
  "params": [
    {
      "deviceName": "deviceName1234",
      "productKey": "1234556554"
    }
  ]
}
```

Response data format when using the Alink protocol

``` {#codeblock_5z3_65k_4kq}
{
  "id": "123",
  "code": 200,
  "data":{}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. You need to define IDs for upstream messages using numbers, and the message IDs must be unique within the device.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This parameter can be left empty.|
|deviceName|String|Name of the sub-device.|
|productKey|String|Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

Error messages

|Error code|Error message|Description|
|:---------|:------------|:----------|
|460|request parameter error|The request parameters are incorrect.​|
|6250|product not found|The specified product to which the sub-device belongs does not exist.|
|6280|devicename not meet specs|The name of the sub-device is invalid. The device name must be 4 to 32 characters in length and can contain letters, digits, hyphens \(-\), underscores \(\_\), at signs \(@\), periods \(.\), and colons \(:\).|

## Notify the gateway to add topological relationships of the connected sub-devices {#section_cn4_zzw_y2b .section}

Downstream

-   Request topic: `/sys/{productKey}/{deviceName}/thing/topo/add/notify`
-   Reply topic: `/sys/{productKey}/{deviceName}/thing/topo/add/notify_reply`

IoT Platform publishes a message to this topic to notify a gateway to add topological relationships of the connected sub-devices. You can use this topic together with the topic that reports new sub-devices to IoT Platform. IoT Platform can subscribe to a data exchange topic to receive the response from the gateway. The data exchange topic is `/{productKey}/{deviceName}/thing/downlink/reply/message`.

Request data format when using the Alink protocol

``` {#codeblock_pko_e28_9ib}
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

``` {#codeblock_s36_1sd_vdb}
{
  "id": "123",
  "code": 200,
  "data": {}
}
```

​Parameter description​

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|params|Object|Request parameters. This parameter can be left empty.|
|method|String|Request method. The value is `thing.topo.add.notify`.|
|deviceName|String|Name of the sub-device.|
|productKey|String|Product ID of the sub-device.|
|code|Integer|Result code. A value of 200 indicates the request is successful.|

## Notify the gateway about topological relationship change {#section_m5p_6up_xvp .section}

When you add, delete, disable, or enable sub-devices on IoT Platform, IoT Platform will change the topological relationships accordingly and will send notifications to the gateway.

The gateway subscribes to the topic: `/sys/{productKey}/{deviceName}/thing/topo/change` for topological relationship change notifications.

Message format when using the Alink protocol

``` {#codeblock_eu0_vcq_p53}
{
    "id":"123",
    "version":"1.0",
    "params":{
        "status":0,  //0:create  1:delete 2-enable  8-disable
        "subList":[{
            "productKey":"a1hRrzD****",
            "deviceName":"abcd"
        }]
    }, 
  "method":"thing.topo.change"  
}
```

|Parameter|Type|Description|
|:--------|:---|:----------|
|id|String|Message ID. IoT Platform generates IDs for downstream messages.|
|version|String|Protocol version. Currently, the value can only be 1.0.|
|method|String|Request method. The value is `thing.topo.change`.|
|params|Object|Message content parameters, including status and sublist.|
|status|Integer|The operation result of topological relationship change. -   0: Create
-   1: Delete
-   2: Disable
-   8: Enable

 |
|deviceName|String|Name of the sub-device.|
|productKey|String|Product ID of the sub-device.|

Response data format when using the Alink protocol

``` {#codeblock_ext_311_b4x}
{
    "id":"123",
    "code":200,
    "message":"success",
    "data":{}
}
```

