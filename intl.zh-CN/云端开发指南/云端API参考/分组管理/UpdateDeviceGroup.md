# UpdateDeviceGroup {#reference_fsq_1kx_kfb .reference}

调用该接口修改分组信息。

## 请求参数 {#section_j3d_jhd_lfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：UpdateDeviceGroup。|
|GroupId|String|是|分组ID。分组的全局唯一标识符。|
|GroupDesc|String|否|修改后的分组描述。长度限制为100字符（一个中文汉字占一个字符）。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_tlw_m12_lfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_w3q_v12_lfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=UpdateDeviceGroup
&GroupId=W16X8TvdosecZu91
&GroupDesc=test2
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"B78B4FD1-AE89-417B-AD55-367EBB0C6759",
    "Success":true
}
```

