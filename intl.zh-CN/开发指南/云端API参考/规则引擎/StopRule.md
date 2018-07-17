# StopRule {#reference_uhd_zjd_xdb .reference}

调用该接口停止指定的规则。

## 请求参数 {#section_xmq_cx1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：StopRule。|
|RuleId|Long|是|指要停止的规则ID。|

## 返回参数 {#section_tbn_hx1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

## 示例 {#section_krt_jx1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=StopRule
&RuleId=10000
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"9A2F243E-17FE-4846-BAB5-D02A25155AC4",
    "Success":true
}
```

