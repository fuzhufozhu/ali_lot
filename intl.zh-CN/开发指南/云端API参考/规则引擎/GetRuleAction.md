# GetRuleAction {#reference_hvp_vjd_xdb .reference}

调用该接口查询指定规则动作的详细信息。

## 请求参数 {#section_qq3_jp1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetRuleAction。|
|ActionId|Long|是|要查询的规则动作ID。|

## 返回参数 {#section_dy1_gq1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|RuleActionInfo|RuleActionInfo|调用成功时，返回的规则动作详细信息。详情参见[RuleActionInfo](#table_nct_lq1_ydb)。|

|名称|类型|描述|
|:-|:-|:-|
|Id|Long|规则动作ID。|
|RuleId|Long|该规则动作对应的规则ID。|
|Type|String| 规则动作类，取值：

 REPUBLISH：转发到另一个topic。

 OTS：存储到表格存储。

 MNS：发送消息到消息服务。

 ONS：发送数据到消息队列。

 HiTSDB：存储到高性能时间序列数据库。

 FC：发送数据到函数计算。

 RDS：存储数据到云数据库中。

 |
|Configuration|String|该规则动作的配置信息。|

## 示例 {#section_dwh_vq1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetRuleAction
&ActionId=10001
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "8FC9E36B-E0DC-4802-84EE-184E255B4E95",
    "RuleActionInfo": {
        "Configuration": "{\"endPoint\":\"http://ons.aliyun.com:8080/rocketmq/nsaddr4client\",\"regionName\":\"cn-shanghai\",\"topic\":\"test_hhf\",\"uid\":\"3216***\"}",
        "Id": 10001,
        "RuleId": 1000,
        "Type": "ONS"
    },
    "Success": true
}
```

