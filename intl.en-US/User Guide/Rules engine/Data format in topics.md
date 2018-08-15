# Data format in topics {#concept_ap3_lql_b2b .concept}

Using rules engine, you need to write SQL statement to process data stored in topics. In what format data is stored in these topics is therefore important for writing the SQL statement. For IoT Platform Basic topics, data format is defined by yourself. For IoT Platform Pro topics, some are customized and some are pre-defined. For those whose data format is pre-defined, you should process data strictly accord to the format. This article explains the pre-defined data format.

## Report device properties {#section_jrb_lrl_b2b .section}

Get device property from this topic.

Topic：`/sys/{productKey}/{deviceName}/thing/event/property/post` 

Data format:

```
{
"iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"productKey": "1234556554",
"deviceName": "deviceName1234",
"gmtCreate":1510799670074,
"deviceType":"Ammeter",
"items"：{
"Power": {
"value": "on",
"time": 1510799670074
},
"Position":{
"time":1510292697470,
"value":{
"latitude":39.90,
"longitude":116.38
}
}
}
}
```

Parameter description:

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The unique identifier of the device within IoT Platform|
|productKey|String|The unique identifier of the product|
|deviceName|String|The name of the device|
|deviceType|String|The type of the device|
|items|Object|Device Data|
|Power|String|A property name. Refer to TSL for all this product's property names.|
|Position|String|A property name. Refer to TSL for all this product's property names.|
|value|Defined in TSL|Property values|
|time|Long|Property generated time. Use Cloud end time if not reported by device.|
|gmtCreate|Long|The time when message data starts to flow|

## Report device event {#section_rlf_fsl_b2b .section}

Get device event from this topic.

Topic：`/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post` 

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
"latitude":39.90,
"longitude":116.38
}
},
"time":1510799670074
}
```

Parameter description:

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The unique identifier of the device within IoT Platform|
|productKey|String|The unique identifier of the product|
|deviceName|String|The name of the device|
|type |String|Event type. Refer to TSL for details.|
|value|Object|Parameters for the event|
|Power|String|A parameter name for the event|
|Position|String|A parameter name for the event|
|time|Long|Use Cloud end time if not reported by device.|
|gmtCreate|Long|The time when message data starts to flow|

## Gateway discovers sub-devices {#section_d5t_qsl_b2b .section}

In some cases, the gateway can discover sub-devices and report their information. The sub-devices' information is reported using this topic.

Topic：`/sys/{productKey}/{deviceName}/thing/list/found` 

Data format:

```

{
"gwIotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"gwProductKey":"1234556554",
"gwDeviceName":"deviceName1234", 
"devices":[{
"productKey":"12345565569",
"deviceName": "deviceName1234",
"iotId":"4z819VQHk6VSLmmBJfrf00107ee201"
}]
}
```

Parameter description:

|Parameter|Type|Description|
|---------|----|-----------|
|gwIotId|String|The unique identifier of the gateway device within the Platform|
|gwProductKey|String|The unique identifier of the gateway product|
|gwDeviceName|String|The name of the gateway device|
|iotId|String|The unique identifier of the sub-device within IoT Platform|
|productKey|String|The unique identifier of the sub-device product|
|deviceName|String|The name of the sub-device|

## Instruction execution result {#section_mgr_2tl_b2b .section}

Obtain the device's instruction execution result from this topic when instructions were sent asynchronously from the Cloud. If an error occurs when sending the instructions, you can get error message from this topic.

Topic：`/sys/{productKey}/{deviceName}/thing/downlink/reply/message` 

Data format:

```

{
"gmtCreate": 1510292739881,
"iotId": "4z819VQHk6VSLmmBJfrf00107ee200",
"productKey": "1234556554",
"deviceName": "deviceName1234",
"requestId": 1234,
"code": 200,
"message": "success",
"topic": "/sys/1234556554/deviceName1234/thing/service/property/set",
"data": {}
}
```

Parameter description:

|Parameter|Type|Description|
|---------|----|-----------|
|gmtCreate|Long|UTC timestamp|
|iotId|String|The unique identifier of the device within IoT Platform|
|productKey|String|The product key.|
|deviceName|String|The name of the device|
|RequestId|Long|The identifier of message between Alibaba Cloud and devices|
|code|Integer|The code for result message|
|message|String|The description of the result|
|data|Object|The result reported by the device. For passthrough communication, this result should be converted by script.|

Response messages

|Parameter|Type|Description|
|---------|----|-----------|
|200|success|The request is successful.|
|400|request error|Internal service error occurs when executing the instructions.|
|460|request parameter error|Request parameter error. Verification of input parameter failed.|
|429|too many requests|Too many requests in a short time.|
|9200|device not actived|The device is not activated yet.|
|9201|device offline|The device is offline now.|
|403|request forbidden|The request is prohibited because of an overdue bill.|

## Online offline status {#section_fyq_xsl_b2b .section}

Obtain the online and offline status of devices from this topic.

Topic: `{productKey}/{deviceName}/mqtt/status` 

Data format:

```

{
"productKey": "1234556554",
"deviceName": "deviceName1234",
"gmtCreate":1510799670074,
"deviceType":"Ammeter",
"iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"action":"online",
"status"{
"value": "14",
"time":1510292697471
}
}
```

Parameter description:

|Parameter|Type|Description|
|---------|----|-----------|
|iotId|String|The unique identifier of the device within IoT Platform|
|productKey|String|The unique identifier of the product|
|deviceName|String|The name of the device|
|status|Object|The status of the device|
|value|String|1 represents online and 0 offline.|
|time|Long|The time when the device got online or offline|
|gmtCreate|Long|The time when the message starts to flow|
|action|String|online or offline|

