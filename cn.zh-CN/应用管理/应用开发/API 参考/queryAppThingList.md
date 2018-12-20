# queryAppThingList {#reference_ljp_lfs_qfb .reference}

调用该接口查询应用所绑定的设备列表。

## 请求路径 {#section_kk5_x4t_qfb .section}

该接口的请求路径：/app/thing/list

## 请求参数 {#section_rbv_cpt_qfb .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|基础参数|-|是|调用应用管理API所需基础参数，请参见[基础参数](cn.zh-CN/应用管理/应用开发/API 参考/基础参数.md#)。|
|tagList|JSON|否|要查询的设备的标签列表。|
|pageNo|Integer|否|指定从返回结果的第几页开始显示。默认值为1。|
|pageSize|Integer|否|每页的记录数，最大值为200。|
|productKeyList|Array|否|要查询的设备所对应的产品 Key 列表。|
|categoryKeyList|Array|否|要查询的设备对应的品类列表。|

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
|data|Object|调用成功时，返回的数据。请参见下表 PaginationResult。|

|参数|类型|描述|
|:-|:-|:-|
|total|Integer|总记录数。|
|pageNo|Integer|当前页。|
|pageSize|Integer|每页记录数。|
|data|Array|返回的设备信息。请参见下表SimpleDeviceInfo。|

|名称|类型|描述|
|:-|:-|:-|
|iotId|String|设备ID。设备的唯一标识符。|
|productName|String|设备所属产品名称。|
|productKey|String|设备所属产品唯一标识符。|
|deviceName|String|设备名称。|
|nodeType|Integer|设备节点类型。|
|activeTime|String|设备激活时间。|
|createTime|String|设备创建时间。|
|lastOnlineTime|String|设备最后在线时间。|
|childDeviceCount|Long|子设备数量。|
|status|String|设备状态。取值：-   UNACTIVE：设备未激活。
-   ONLINE：设备在线。
-   OFFLINE：设备离线。
-   DISABLE：设备已禁用。
-   ACTIVE：设备已激活。

|
|utcLastOnlineTime|String|设备最后在线的 UTC 时间。|
|categoryKey|String|设备所属产品的品类信息。|
|utcCreateTime|String|设备创建的 UTC 时间。|
|aliyunCommodityCode|String|设备所属产品的版本。取值：-   iothub：物联网平台基础版 。
-   iothub\_senior：物联网平台高级版。

|
|utcActiveTime|String|设备激活的 UTC 时间。|

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
        "pageNo":1,
        "pageSize":20,
        "productKeyList":[
            al22rf3g2
        ],
        "categoryKeyList":[
            smoketest
        ],
        "tagList":[
            "room1":"test"
        ]
    }
}
```

**返回示例**

```
{
    "id":"bded4128dc454a03b3d10c45de17b998",
    "code":200,
    "data":{
        "data":[
            {
                "activeTime":"1540808151005",
                "aliyunCommodityCode":"",
                "categoryKey":"xx",
                "childDeviceCount":1540808151006,
                "createTime":"1540808151006",
                "deviceName":"test0",
                "iotId":"xxxx",
                "lastOnlineTime":"1540808151006",
                "nodeType":1,
                "productKey":"xxx",
                "productName":"name",
                "status":"ACTIVE",
                "utcActiveTime":"",
                "utcCreateTime":"",
                "utcLastOnlineTime":""
            },
            {
                "activeTime":"1540808151006",
                "aliyunCommodityCode":"",
                "categoryKey":"xx",
                "childDeviceCount":1540808151006,
                "createTime":"1540808151006",
                "deviceName":"test1",
                "iotId":"xxxx",
                "lastOnlineTime":"1540808151006",
                "nodeType":1,
                "productKey":"xxx",
                "productName":"name",
                "status":"ACTIVE",
                "utcActiveTime":"",
                "utcCreateTime":"",
                "utcLastOnlineTime":""
            }
        ],
        "pageNo":1,
        "pageSize":20,
        "total":2
    },
    "message":"success",
    "localizedMsg": null
}
```

