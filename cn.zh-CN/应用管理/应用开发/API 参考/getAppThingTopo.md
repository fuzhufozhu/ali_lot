# getAppThingTopo {#reference_ljp_lfs_qfb .reference}

调用该接口获取物（设备）的拓扑关系。

## 请求路径 {#section_kk5_x4t_qfb .section}

该接口的请求路径：/app/thing/topo/get

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
|pageNo|Integer|是|指定从返回结果中的第几页开始显示。|
|pageSize|Integer|是|指定返回结果中，每页显示的记录数。|

## 返回参数 {#section_cl2_rst_qfb .section}

|参数|类型|描述|
|:-|:-|:-|
|id|String|物联网平台为该请求生成的唯一标识符。|
|code|Integer|返回的结果状态码。-   调用成功，则返回码为 200。
-   若调用失败，则返回具体错误码。错误码详情参见[通用错误码](cn.zh-CN/应用管理/应用开发/API 参考/通用错误码.md#)。

|
|message|String|调用结果的英文描述。-   当返回的 code为200时，message 为success。
-   当返回的code为其他错误码时，message为该错误码对应的具体描述。具体信息，请参见[通用错误码](cn.zh-CN/应用管理/应用开发/API 参考/通用错误码.md#)。

|
|localizedMsg|String|本地化语言的调用结果描述，目前为空。|
|data|Object|调用成功时， 返回的数据。详情请参见下表 PaginationResult 描述。|

|名称|类型|描述|
|:-|:-|:-|
|total|Integer|总记录数。|
|pageNo|Integer|当前页。|
|pageSize|Integer|每页记录数。|
|data|Array|详情请参见下表 SimpleDeviceDTO 。|

|名称|类型|描述|
|:-|:-|:-|
|iotId|String|设备ID。物联网平台为设备颁发的ID，作为该设备的唯一标识符。|
|productKey|String|设备所属产品的Key。|
|deviceName|String|设备名称。|
|deviceSecret|String|设备秘钥。|

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
        "pageNo":1,
        "pageSize":20
    }
}
```

**返回示例**

```
{
    "id":"bded4128dc454a03b3d10c45de17b867",
    "code":200,
    "data":{
        "pageNo":1,
        "pageSize":20,
        "total":2,
        "data":[
            {
                "deviceName":"test0",
                "deviceSecret":"xxxxx",
                "iotId":"xxxxxxxx",
                "productKey":"xxx"
            },
            {
                "deviceName":"test1",
                "deviceSecret":"xxxxx",
                "iotId":"xxxxxxxx",
                "productKey":"xxx"
            }
        ]
    },
    "message":"success",
    "localizedMsg": null
}
```

