# CreateDeviceGroup {#reference_fbz_b3x_kfb .reference}

调用该接口新建分组。

## 请求参数 {#section_j5s_3kx_kfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：CreateDeviceGroup。|
|SuperGroupId|String|否|父组ID。若要创建的是一级分组，则不传入此参数。

|
|GroupName|String|是|分组名称。名称可包含中文汉字、英文字母、数字和下划线（\_）。长度范围 4 - 30 字符（一个中文汉字占二个字符）。|
|GroupDesc|String|否|分组描述。长度限制为100字符（一个中文汉字占一个字符）。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_kqy_ymx_kfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|请参考下表Group。|

|名称|类型|描述|
|:-|:-|:-|
|GroupName|String|分组名称。|
|UtcCreate|Date|创建时间。|
|GroupDesc|String|分组描述。|
|GroupId|String|分组ID，系统为分组生成的全局唯一标识符。|

## 示例 {#section_z4x_sxx_kfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=CreateDeviceGroup
&GroupDesc=Group test
&GroupName=grouptest
&公共请求参数
```

**返回示例**

```
{
    "Data":{
        "GroupDesc":"Group test",
        "GroupName":"grouptest",
        "UtcCreate":"2018-10-17T11:19:31.000Z",
        "GroupId":"HtMLECKbdJQLIcbY"
    },
    "RequestId":"4D6D7F71-1C94-4160-8511-EFF4B8F0634D",
    "Success":true
}
```

