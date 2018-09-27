# DeleteRuleAction {#reference_e11_5jd_xdb .reference}

调用该接口删除指定的规则动作。

## 请求参数 {#section_wsx_km1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteRuleAction。|
|ActionId|Long|是|要删除的规则动作ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_p41_rm1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

## 示例 {#section_n2w_5m1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteRuleAction
&ActionId=10001
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "8FC9E36B-E0DC-4802-84EE-184E255B4E95",
    "Success": true
}
```

