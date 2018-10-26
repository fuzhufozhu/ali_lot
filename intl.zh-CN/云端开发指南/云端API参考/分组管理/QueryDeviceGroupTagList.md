# QueryDeviceGroupTagList {#reference_hvj_tjx_kfb .reference}

调用该接口查询分组标签列表。

## 请求参数 {#section_y5y_wmf_lfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDeviceGroupTagList。|
|GroupId|String|是|分组ID，分组的全局唯一标识符。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_ec4_fnf_lfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的标签信息。请参见下表GroupTagInfo。|

|名称|类型|描述|
|:-|:-|:-|
|TagKey|String|标签键。|
|TagValue|String|标签值。|

## 示例 {#section_dxf_rnf_lfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceGroupTagList
&GroupId=W16X8TvdosecZu91
&公共请求参数
```

**返回示例**

```
{
    "Data":{
        "GroupTagInfo":[
            {
                "TagValue":"bulb",
                "TagKey":"room1"
            }
        ]
    },
    "RequestId":"214154FF-9D47-4E3F-AAAD-F4CE67F41060",
    "Success":true
}
```

