# QueryDeviceGroupList {#reference_j4r_p3x_kfb .reference}

调用该接口分页查询分组列表。

## 请求参数 {#section_gcz_dwc_lfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDeviceGroupList。|
|GroupName|String|否|分组名称。-   传入分组名称，则根据名称进行查询。支持分组名称模糊查询。
-   若不传入此参数，则进行全量分组查询。

|
|SuperGroupId|String|否|父组ID。查询某父组下的子分组列表时，需传入此参数。|
|PageSize|Integer|否|每页记录数。最大值是200。默认值是10。|
|CurrentPage|Integer|否|指定从返回结果中的第几页开始显示。默认值为1。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_dmv_vcd_lfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页记录数。|
|CurrentPage|Integer|当前页号。|
|Total|Integer|总记录数。|
|Data|Data|调用成功时，返回的分组信息。请参见下表GroupInfo。**说明：** 返回的分组信息按照分组创建时间倒序排列。

|

|名称|类型|描述|
|:-|:-|:-|
|GroupName|String|分组名称。|
|UtcCreate|Date|分组创建时间。|
|GroupDesc|String|分组描述。|
|GroupId|String|分组ID。|

## 示例 {#section_uls_dfd_lfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceGroupList
&PageSize=10
&CurrentPage=1
&公共请求参数
```

**返回示例**

```
{
    "PageCount":3,
    "Data":{
        "GroupInfo":[
            {
                "GroupName":"test1",
                "UtcCreate":"2018-10-09T02:58:34.000Z",
                "GroupId":"Kzt9FD8wje8oW64y"
            },
            {
                "GroupName":"test2",
                "UtcCreate":"2018-10-09T02:56:40.000Z",
                "GroupId":"0ayrSQ3DSd7uXXXC"
            },
            {
                "GroupDesc":"Test",
                "GroupName":"test3",
                "UtcCreate":"2018-09-16T05:38:27.000Z",
                "GroupId":"oWXlIQeFZtzCNsAV"
            },
            {
                "GroupName":"ylv0915",
                "UtcCreate":"2018-09-15T04:51:56.000Z",
                "GroupId":"SfEiVapLPUjBVvSq"
            },
            {
                "GroupName":"ydlv",
                "UtcCreate":"2018-09-14T14:35:51.000Z",
                "GroupId":"z2S2h9NsDTZmH8MR"
            },
            {
                "GroupName":"ldh_group_3",
                "UtcCreate":"2018-09-14T12:26:20.000Z",
                "GroupId":"chn5fkjinXGc7pGl"
            },
            {
                "GroupDesc":"ddd",
                "GroupName":"ylvgisbim",
                "UtcCreate":"2018-09-14T11:41:20.000Z",
                "GroupId":"ncUZ8DjWYaB9RxlO"
            },
            {
                "GroupName":"abc",
                "UtcCreate":"2018-09-14T09:14:30.000Z",
                "GroupId":"zpdvwxzBdt4FljYf"
            },
            {
                "GroupName":"test11",
                "UtcCreate":"2018-09-14T07:22:39.000Z",
                "GroupId":"BTaudF16X2xKQBNa"
            },
            {
                "GroupName":"testy",
                "UtcCreate":"2018-09-14T01:58:06.000Z",
                "GroupId":"PrTm3VOeggPwRUKQ"
            }
        ]
    },
    "PageSize":10,
    "RequestId":"BEFCA316-D6C7-470C-81ED-1FF4FFD4AA0D",
    "CurrentPage":1,
    "Success":true,
    "Total":24
}
```

