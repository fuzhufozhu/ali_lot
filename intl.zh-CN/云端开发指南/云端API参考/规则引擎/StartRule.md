# StartRule {#reference_m1j_yjd_xdb .reference}

调用该接口启动指定的规则。

## 请求参数 {#section_cyy_v51_ydb .section}

**说明：** 启动规则前需要确认该规则已经配置了SQL。配置SQL，参考[CreateRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRule.md#)接口。

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：StartRule。|
|RuleId|Long|是|要启动的规则ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_m43_gv1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

## 示例 {#section_w1j_jw1_ydb .section}

**请求参数**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=StartRule
&RuleId=1000
&公共请求参数
```

**返回参数**

```
{
    "RequestId":"9A2F243E-17FE-4846-BAB5-D02A25155AC4",
    "Success":true
}
```

