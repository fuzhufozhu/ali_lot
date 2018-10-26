# SetDeviceGroupTags {#reference_frf_vjx_kfb .reference}

调用该接口添加、更新、或删除分组标签。

## 请求参数 {#section_a2c_24f_lfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：SetDeviceGroupTags|
|GroupId|String|是|分组ID，分组的全局唯一标识符。|
|TagString|String|是|JSON格式的标签数据。TagString由标签的key和value组成，key和value均不能为空。多个标签以英文逗号间隔。如，`TagString="[{"tagKey":"hh","tagValue":"rr"}]"`。若更新已有标签，新的标签value值将覆盖原来的值。

若要删除某个标签，则不传入该标签的key和value即可。

|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_nsj_cpf_lfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_ysn_gpf_lfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=SetDeviceGroupTags
&GroupId=W16X8TvdosecZu91
&TagString=%5B%7B%22tagKey%22%3A%22sdfsd%22%2C%22tagValue%22%3A%22%E6%98%AF%E9%9D%9E%E5%BE%97%E5%A4%B1%22%7D%5D
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"12CFDAF1-99D9-42E0-8C2F-F281DA5E8953",
    "Success":true
}
```

