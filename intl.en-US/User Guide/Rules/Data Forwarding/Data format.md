# Data format {#concept_ap3_lql_b2b .concept}

If you want to use rules engine to forward data, you need to write a SQL statement to process data using message topics. Therefore, the format in which data is stored in these topics must be able to be parsed by SQL statements. For IoT Platform Basic edition topics, the data format is defined manually. For IoT Platform topics, the data format of custom topics is defined manually, and the data format of system topics is pre-defined by the system. For scenarios where the data format is pre-defined, data is strictly processed according to the format. This topic explains the pre-defined data format of system defined topics.

## Messages about device properties reported by devices {#section_jrb_lrl_b2b .section}

By using the following topic, you can obtain the device properties reported by devices.

Topic：`/sys/{productKey}/{deviceName}/thing/event/property/post` 

Data format:

```
{
    "iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
    "productKey":"1234556554",
    "deviceName":"deviceName1234",
    "gmtCreate":1510799670074,
    "deviceType":"Ammeter",
    "items":{
        "Power":{
            "value":"on",
            "time":1510799670074
        },
        "Position":{
            "time":1510292697470,
            "value":{
                "latitude":39.9,
                "longitude":116.38
            }
        }
    }
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|iotId|String|The unique identifier of the device.|
|productKey|String|The unique identifier of the product to which the device belongs.|
|deviceName|String|The name of the device.|
|deviceType|String|The node type of the device.|
|items|Object|Device data.|
|Power|String|The property name. See the TSL description of the product for all the property names.|
|Position|String|The property name. See the TSL description of the product for all the property names.|
|value|Defined in TSL|Property values|
|time|Long|The time when the property is created. If the device does not report the time, the time when the property is generated on the cloud will be used.|
|gmtCreate|Long|The time when the message is generated.|

## Messages about events reported by devices {#section_rlf_fsl_b2b .section}

By using the following topic, you can obtain event information reported by devices.

Topic: `/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post` 

Data format:

```
{
    "identifier":"BrokenInfo",
    "Name": "Damage rate report ",
    "type":"info",
    "iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
    "productKey":"X5eCzh6fEH7",
    "deviceName":"5gJtxDVeGAkaEztpisjX",
    "gmtCreate":1510799670074,
    "value":{
        "Power": "on",
        "Position":{
            "latitude":39.9,
            "longitude":116.38
        }
    },
    "time":1510799670074
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|iotId|String|The unique identifier of the device.|
|productKey|String|The unique identifier of the device product.|
|deviceName|String|The name of the device.|
|type|String|Event type. See the TSL of the product for details.|
|value|Object|Parameters of the event.|
|Power|String|The parameter name of the event.​|
|Position|String|The parameter name of the event|
|time|Long|The time when the event is generated. If the device does not report the time, the time recorded on the cloud will be used.|
|gmtCreate|Long|The time when the message is generated.|

## Device lifecycle change messages {#section_ijz_y2n_qfb .section}

By using the following topic, you can obtain messages about device creation and deletion, and about devices being enabled and disabled.

Topic: `/sys/{productKey}/{deviceName}/thing/lifecycle`

Data format:

```
{
"action" : "create|delete|enable|disable",
"iotId" : "4z819VQHk6VSLmmBJfrf00107ee200",
"productKey" : "X5eCzh6fEH7",
"deviceName" : "5gJtxDVeGAkaEztpisjX",
"deviceSecret" : "", 
"messageCreateTime": 1510292739881 
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|action|String| -   create: Create devices.
-   delete: Delete devices.
-   enable: Enable devices.
-   disable: Disable devices.

 |
|iotId|String|The unique identifier of the device.|
|productKey|String|The unique identifier of the product.|
|deviceName|String|The name of the device.|
|deviceSecret|String|The device secret. This parameter is only included when the value of action is create.|
|messageCreateTime|Integer|The timestamp when the message is generated, in milliseconds.|

## Device topological relationship update messages {#section_khs_ffn_qfb .section}

By using the following topic, you can obtain messages about topological relationship creation and removal between sub-devices and gateways.

Topic: `/sys/{productKey}/{deviceName}/thing/topo/lifecycle`

Data format:

```
{
"action" : "add|remove|enable|disable",
"gwIotId": "4z819VQHk6VSLmmBJfrf00107ee200",
"gwProductKey": "1234556554",
"gwDeviceName": "deviceName1234",
"devices": [
        {
"iotId": "4z819VQHk6VSLmmBJfrf00107ee201",
"productKey": "12345565569",
"deviceName": "deviceName1234"
       }
    ],

"messageCreateTime": 1510292739881
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|action|String| -   add: Add topological relationships.
-   remove: Delete topological relationships.
-   enable: Enable topological relationships.
-   disable: Disable topological relationships.

 |
|gwIotId|String|The unique identifier of the gateway device.|
|gwProductKey|String|The unique identifier of the gateway product.|
|gwDeviceName|String|The name of the gateway device.|
|devices|Object|The sub-devices whose topological relationship with the gateway will be updated.|
|iotId|String|The unique identifier of the sub-device.|
|productKey|String|The unique identifier of the sub-device product.|
|deviceName|String|The name of the sub-device.|
|messageCreateTime|Integer|The timestamp when the message is generated, in milliseconds.|

## Messages about detected sub-devices reported by gateways {#section_d5t_qsl_b2b .section}

In some cases, gateways can detect sub-devices and report their information. By using the following topic, you can obtain the sub-device information reported by gateways.

Topic: `/sys/{productKey}/{deviceName}/thing/list/found` 

Data format:

```
{
    "gwIotId":"4z819VQHk6VSLmmBJfrf00107ee200",
    "gwProductKey":"1234556554",
    "gwDeviceName":"deviceName1234",
    "devices":[
        {
            "iotId":"4z819VQHk6VSLmmBJfrf00107ee201",
            "productKey":"12345565569",
            "deviceName":"deviceName1234"
        }
    ]
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|gwIotId|String|The unique identifier of the gateway device.|
|gwProductKey|String|The unique identifier of the gateway product.|
|gwDeviceName|String|The name of the gateway device.|
|devices|Object|The sub-devices that are detected by the gateway.|
|iotId|String|The unique identifier of the sub-device.|
|productKey|String|The unique identifier of the sub-device product.|
|deviceName|String|The name of the sub-device.|

## Devices return result data to the cloud {#section_mgr_2tl_b2b .section}

By using the following topic, you can obtain request execution results from devices when you send operation requests to devices using an asynchronous method. If an error occurs when sending the request, you will receive an error message from this topic.

Topic: `/sys/{productKey}/{deviceName}/thing/downlink/reply/message` 

Data format:

```
{
    "gmtCreate":1510292739881,
    "iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
    "productKey":"1234556554",
    "deviceName":"deviceName1234",
    "requestId":1234,
    "code":200,
    "message":"success",
    "topic":"/sys/1234556554/deviceName1234/thing/service/property/set",
    "data":{

    }
}
```

Parameter descriptions

|Parameter|Type |Description|
|---------|-----|-----------|
|gmtCreate|Long|The timestamp when the message is generated.|
|iotId|String|The unique identifier of the device.|
|productKey|String|The unique identifier of the product.|
|deviceName|String|The name of the device.|
|requestId|Long|The request message ID.|
|code|Integer|The code for the result message.|
|message|String|The description of the result.|
|data|Object|The result data reported by the device. For pass-through communication, the result data will be converted by the parsing script.|

Response information:

|Parameter|Message|Description|
|---------|-------|-----------|
|200 |success |The request is successful.|
|400|request error|Internal service error.|
|460|request parameter error |The request parameters are invalid. The device has failed input parameter verification.|
|429|too many requests |Too many requests in a short time.|
|9200|device not activated|The device is not activated yet.|
|9201|device offline|The device is offline now.|
|403|request forbidden|The request is prohibited because of an overdue bill.|

## Messages about device status {#section_fyq_xsl_b2b .section}

By using the following topic, you can obtain the online and offline status of devices.

Topic: `{productKey}/{deviceName}/mqtt/status` 

Data format:

```
{
    "productKey":"1234556554",
    "deviceName":"deviceName1234",
    "gmtCreate":1510799670074,
    "deviceType":"Ammeter",
    "iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
    "action":"online|offline",
    "status":{
        "value":"1",
        "time":1510292697471
    }
}
```

Parameter descriptions:

|Parameter|Type |Description|
|---------|-----|-----------|
|iotId|String|The unique identifier of the device.|
|productKey|String|The unique identifier of the device product.|
|deviceName|String|The name of the device.|
|status|Object|The status of the device.|
|Value|String|1: online; 0: offline.|
|time|Long|The time when the device got online or offline.|
|gmtCreate|Long|The time when the message is generated.|
|action|String|The action of device status change: go online or go offline.|

