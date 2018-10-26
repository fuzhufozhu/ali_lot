# QueryDeviceGroupInfo {#reference_brn_n3x_kfb .reference}

调用该接口查询分组详情。

## 请求参数 {#section_gkf_ycy_kfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDeviceGroupInfo。|
|GroupId|String|是|分组ID，分组的全局唯一标识符。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_qsy_ldy_kfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的分组详细信息数据。请参考下表GroupInfo。|

|名称|类型|描述|
|:-|:-|:-|
|GroupName|String|分组名称。|
|UtcCreate|String|创建时间。|
|DeviceOnline|Integer|在线设备数量。|
|DeviceActive|Integer|激活设备数量。|
|DeviceCount|Integer|设备总数。|
|GroupId|String|分组ID。|
|GroupDesc|String|分组描述。|

## 示例 {#section_z5l_42y_kfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceGroupInfo
&GroupId=tDQvBJqbUyHskDse
&公共请求参数
```

**返回示例**

```
{
    "Data":{
        "DeviceOnline":0,
        "DeviceActive":1,
        "GroupName":"yanglv",
        "DeviceCount":10,
        "UtcCreate":"2018-09-14T14:35:51.000Z",
        "GroupId":"tDQvBJqbUyHskDse"
    },
    "RequestId":"7411716B-A488-4EEB-9AA0-6DB05AD2491F",
    "Success":true
}
```

