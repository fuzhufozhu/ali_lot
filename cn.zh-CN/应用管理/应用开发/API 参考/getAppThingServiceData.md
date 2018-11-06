# getAppThingServiceData {#reference_ljp_lfs_qfb .reference}

调用该接口获取物（设备）的标签列表。

## 请求路径 {#section_kk5_x4t_qfb .section}

该接口的请求路径：/app/thing/service/data/get

## 请求参数 {#section_rbv_cpt_qfb .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|基础参数|-|是|调用应用管理API所需基础参数，请参见[基础参数](cn.zh-CN/应用管理/应用开发/API 参考/基础参数.md#)。|
|iotId|String|否|设备ID，设备的唯一标识。**说明：** 如果传入该参数，则无需传入productKey和deviceName。iotId作为设备唯一标识符，和productKey与DeviceName组合是一一对应的关系。如果您同时传入iotId和productKey与deviceName组合，则以iotId为准。

|
|productKey|String|否|设备所隶属的产品Key。**说明：** 如果传入该参数，需同时传入deviceName。

|
|deviceName|String|否|设备名称。**说明：** 如果传入该参数，需同时传入productKey。

|
|identifier|String|是|服务的唯一标识符。|
|start|Long|是|指定要查询的服务记录的开始时间。|
|end|Long|是|指定要查询的服务记录的结束时间。|
|pageSize|Integer|是|每页所显示的记录数。|
|ordered|Boolean|是|查询顺序。取值：-   true：按顺序查询。
-   false：按倒序查询。

|

## 返回参数 {#section_cl2_rst_qfb .section}

|参数|类型|描述|
|:-|:-|:-|
|id|String|物联网平台为该请求生成的唯一标识符。|
|code|Integer|返回的结果状态码。-   调用成功，则返回码为 200。
-   若调用失败，则返回具体错误码。错误码详情参见[通用错误码](cn.zh-CN/应用管理/应用开发/API 参考/通用错误码.md#)。

|
|message|String|调用结果的英文描述。-   当返回的 code为200时，message 为success。
-   当返回的code为其他错误码时，message为该错误码对应具体的描述。具体信息，请参见[通用错误码](cn.zh-CN/应用管理/应用开发/API 参考/通用错误码.md#)。

|
|localizedMsg|String|本地化语言的调用结果描述，目前为空。|
|data|Object|调用成功时， 返回的数据。详情请参见下表 ServiceDataInfoDTO。|

|参数|类型|描述|
|:-|:-|:-|
|nextValid|Boolean|表示下一页是否可用。取值：-   true：可用。
-   false：不可用。

|
|nextTime|Long|下一页中的服务记录的起始时间。|
|list|Array|服务事件列表。具体信息参见下表 ServiceInfoDTO。|

|参数|类型|描述|
|:-|:-|:-|
|name|String|服务名称。|
|time|Long|服务执行的时间。|
|outputData|String|服务的输出参数，map 格式的字符串，结构为 key:value。|
|inputData|String|服务的输入参数，map 格式的字符串，结构为 key:value。|
|identifier|String|服务的标识符。|

## 示例 {#section_qs1_zxt_qfb .section}

**请求示例**

```
{
    "id":"bded4128dc454a03b3d10c45de17b863",
    "version":"1.0",
    "request":{
        "apiVer":"1.0.0"
    },
    "params":{
        "iotId":"D95D242941CE821ECCE4F31A2697",
        "identifier":"xxx",
        "start":162335465718,
        "end":165168906691,
        "pageSize":10,
        "ordered":true
    }
}
```

**返回示例**

```
{
    "id":"bded4128dc454a03b3d10c45de17b877",
    "code":200,
    "data":{
        "list":[
            {
                "identifier":"identifier0",
                "name":"test0",
                "time":1540804101583,
                "inputData":"key:value",
                "outputData":"key:value"
            },
            {
                "identifier":"identifier1",
                "name":"test1",
                "time":1540804101584,
                "inputData":"key:value",
                "outputData":"key:value"
            }
        ],
        "nextTime":1540804101585,
        "nextValid":true
    },
    "message":"success",
    "localizedMsg": null
}
```

