# ListRuleActions {#reference_jtm_xjd_xdb .reference}

调用该接口查询指定规则下的所有规则动作列表。

## 请求参数 {#section_e2c_4t1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：ListRuleActions。|
|RuleId|String|是|要查询的规则ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_cyf_wt1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|RuleActionList|[RuleActionInfo](intl.zh-CN/云端开发指南/云端API参考/规则引擎/ListRuleActions.md#table_yp5_b51_ydb)|调用成功时，返回的规则动作信息列表。详情参见下表RuleActionInfo。|

|名称|类型|描述|
|:-|:-|:-|
|Id|Long|规则动作ID。|
|RuleId|Long|该规则动作对应的规则ID。|
|Type|String| 规则动作类型。取值：

 REPUBLISH：转发到另一个topic。

 OTS：存储到表格存储。

 MNS：发送消息到消息服务。

 FC：发送数据到函数计算。

 RDS：存储数据到云数据库中。

 |
|Configuration|String|该规则动作的配置信息。|

## 示例 {#section_uz3_j51_ydb .section}

**请求参数**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListRuleActions
&RuleId=10000
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "RuleActionList": [{
        "Configuration": "{\"topic\":\"/a1GM***/bike/get\",\"uid\":\"3216***\"}",
        "Id": 10001,
        "RuleId": 1000,
        "Type": REPUBLISH
    },
    {
        "Configuration": "{\"endPoint\":\"http://ons.aliyun.com:8080/rocketmq/nsaddr4client\",\"regionName\":\"cn-shanghai\",\"topic\":\"test_ons\",\"uid\":\"3216***\"}",
        "Id": 10002,
        "RuleId": 1000,
        "Type": ONS
    }],
    "Success": true
}
```

