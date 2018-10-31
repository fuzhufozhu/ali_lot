# DeleteRule {#reference_fmd_tjd_xdb .reference}

调用该接口删除指定的规则。

## 请求参数 {#section_e1p_5l1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteRule。|
|RuleId|Long|是|要删除的规则ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_rvv_jl1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_v5b_bm1_ydb .section}

**请求参数**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteRule
&RuleId=1000
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "A8F48485-44B9-40D8-A56D-F716F384F387",
    "Success": true
}
```

