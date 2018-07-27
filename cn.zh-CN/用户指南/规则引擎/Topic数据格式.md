# Topic数据格式 {#concept_ap3_lql_b2b .concept}

使用规则引擎，您需要基于Topic编写SQL处理数据。基础版Topic和高级版自定义的Topic，数据格式是您自定义的，物联网平台不做处理。高级版系统默认的Topic，物联网平台定义了Topic格式，此时您需要根据平台定义的数据格式，处理数据。本文将为您讲解高级版系统默认Topic的数据格式。

## 设备属性上报 {#section_jrb_lrl_b2b .section}

通过该Topic获取设备上报的属性信息。

Topic：`/sys/{productKey}/{deviceName}/thing/event/property/post` 

数据格式：

```
{
"iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"productKey":"1234556554",
"deviceName":"deviceName1234",
"gmtCreate":1510799670074,
"deviceType":"Ammeter",
"items"：{
"Power":{
"value":"on",
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

参数说明：

|参数|类型|说明|
|--|--|--|
|iotId|String|设备在平台内的唯一标识|
|productKey|String|产品的唯一标识|
|deviceName|String|设备名称|
|deviceType|String|设备类型|
|items|Object|设备数据|
|Power|String|属性名称，产品所具有的属性名称请参考TSL描述|
|Position|String|属性名称，产品所具有的属性名称请参考TSL描述|
|value|根据TSL定义|属性值|
|time|Long|属性产生时间，如果设备没有上报默认采用云端生成时间|
|gmtCreate|Long|数据流转消息产生时间|

## 设备上报事件 {#section_rlf_fsl_b2b .section}

通过该topic获取设备上报的事件信息。

Topic：`/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post` 

数据格式：

```
{
"identifier":"BrokenInfo",
"name":"损坏率上报",
"type":"info",
"iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"productKey":"X5eCzh6fEH7",
"deviceName":"5gJtxDVeGAkaEztpisjX",
"gmtCreate":1510799670074,
"value":{
"Power":"on",
"Position":{
"latitude":39.90,
"longitude":116.38
}
},
"time":1510799670074
}
```

参数说明：

|参数|类型|说明|
|--|--|--|
|iotId|String|设备在平台内的唯一标识|
|productKey|String|产品的唯一标识|
|deviceName|String|设备名称|
|type|String|事件类型，事件类型参考TSL描述|
|value|Object|事件的参数|
|Power|String|事件参数名称|
|Position|String|事件参数名称|
|time|Long|事件产生时间，如果设备没有上报默认采用远端时间|
|gmtCreate|Long|数据流转消息产生时间|

## 网关发现子设备数据流转 {#section_d5t_qsl_b2b .section}

在一些场景中网关能够检测到子设备，并将检测到的子设备信息上报。此时可以通过该Topic获取到上报的信息。

Topic：`/sys/{productKey}/{deviceName}/thing/list/found` 

数据格式：

```

{
"gwIotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"gwProductKey":"1234556554",
"gwDeviceName":"deviceName1234", 
"devices":[{
"productKey":"12345565569",
"deviceName":"deviceName1234",
"iotId":"4z819VQHk6VSLmmBJfrf00107ee201"
}]
}
```

参数说明：

|参数|类型|说明|
|--|--|--|
|gwIotId|String|网关设备在平台内的唯一标识|
|gwProductKey|String|网关产品的唯一标识|
|gwDeviceName|String|网关设备名称|
|iotId|String|子设备在平台内的唯一标识|
|productKey|String|子设备产品的唯一标识|
|deviceName|String|子设备名称|

## 设备下行指令结果数据流转 {#section_mgr_2tl_b2b .section}

通过该Topic可以获取，通过异步方式下发指令给设备，设备进行处理后返回的结果信息。如果下发指令过程中出现错误，也可以通过该Topic得到指令下发的错误信息。

Topic：`/sys/{productKey}/{deviceName}/thing/downlink/reply/message` 

数据格式：

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

参数说明：

|参数|类型|说明|
|--|--|--|
|gmtCreate|Long|UTC时间戳|
|iotId|String|设备在平台内的唯一标识|
|productKey|String|产品Key|
|deviceName|String|设备名称|
|requestId|Long|阿里云产生和设备通信的信息id|
|code|Integer|调用的结果信息|
|message|String|结果信息说明|
|data|Object|设备返回的结果，非透传之间返回设备结果，透传则需要经过脚本转换|

返回信息：

|参数|类型|说明|
|--|--|--|
|200|success|请求成功|
|400|request error|内部服务错误， 处理时发生内部错误|
|460|request parameter error|请求参数错误， 设备入参校验失败|
|429|too many requests|请求过于频繁|
|9200|device not actived|设备没有激活|
|9201|device offline|设备不在线|
|403|request forbidden|请求被禁止，由于欠费导致|

## 设备上下线状态数据流转 {#section_fyq_xsl_b2b .section}

通过该Topic获取设备的上下线状态。

数据流转Topic：`{productKey}/{deviceName}/mqtt/status` 

数据格式：

```

{
"productKey":"1234556554",
"deviceName":"deviceName1234",
"gmtCreate":1510799670074,
"deviceType":"Ammeter",
"iotId":"4z819VQHk6VSLmmBJfrf00107ee200",
"action":"online",
"status"{
"value":"1",
"time":1510292697471
}
}
```

参数说明：

|参数|类型|说明|
|--|--|--|
|iotId|String|设备在平台内的唯一标识|
|productKey|String|产品的唯一标识|
|deviceName|String|设备名称|
|status|Object|设备状态|
|value|String|状态值 1上线，0离线|
|time|Long|设备上下线时间|
|gmtCreate|Long|数据流转消息产生时间|
|action|String|设备状态变更动作，online上线， offline离线|

