# QueryDeviceByTags {#reference_pgk_qck_sfb .reference}

调用该接口通过标签查询设备。

## 请求参数 {#section_sm5_5ck_sfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值 QueryDeviceByTags。|
|PageSize|Integer|否|指定返回结果中每页显示的记录数量，最大值是50。默认值是10。|
|CurrentPage|Integer|否|指定从返回结果中的第几页开始显示。默认值是1。|
|Tag|List<Tag\>|是|设备标签。Tag 包括TagKey和TagValue，分别对应标签的key和value。

支持只传入TagKey进行查询。

Tag详情，请参见下表 Tag。

|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|TagKey|String|是|设备标签的key。|
|TagValue|String|否|设备标签的value。|

## 返回参数 {#section_urg_42k_sfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页面号。|
|Total|Integer|总记录数。|
|Data|Data|调用成功时，返回的设备信息列表。详情参见下表SimpleDeviceInfo。|

|名称|类型|描述|
|:-|:-|:-|
|ProductName|String|产品名称。|
|ProductKey|String|产品Key。|
|DeviceName|String|设备名称。|
|IotId|String|物联网平台为该设备颁发的ID，作为该设备的唯一标识符。|

## 示例 {#section_qbk_fgk_sfb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/&Action=QueryDeviceByTags
&CurrentPage=1
&PageSize=10
&Tag.1.TagKey=dfdfd
&Tag.1.TagValue=dfdfdfdf
&Tag.2.TagKey=ddfdfdf
&Tag.2.TagValue=dfdfdfdfd
&公共请求参数
```

**返回示例**

```
{
    "PageCount": 1,
    "Data": {
        "SimpleDeviceInfo": [
            {
                "DeviceName": "1102jichu02",
                "ProductKey": "a1SM5S1shy1",
                "IotId": "GookTiUcwqRbHosp9Ta10017d3a00",
                "ProductName": "TEST"
            }
        ]
    },
    "PageSize": 10,
    "Page": 1,
    "RequestId": "2B5091E4-32D5-4884-A5B2-2E8E713D84AF",
    "Success": true,
    "Total": 1
}
```

