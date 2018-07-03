# GetRule {#reference_kyt_5jd_xdb .reference}

调用该接口查询指定规则的详细信息。

## 请求参数 {#section_bsw_fn1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetRule。|
|RuleId|Long|是|要查询的规则ID。|

## 返回参数 {#section_dlm_ln1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。ture表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|RuleInfo|RuleInfo|调用成功时，返回的规则详细信息。详情参见[RuleInfo](#table_xgb_yn1_ydb)。|

|名称|类型|描述|
|:-|:-|:-|
|CreateUserId|Long|创建该规则的用户ID。|
|Created|String|该规则的创建时间。|
|DataType|String|该规则的数据类型，取值：JSON和BINARY。|
|Id|Long|规则ID。|
|Modified|String|该规则最近一次被修改的时间。|
|Name|String|规则名称。|
|ProductKey|String|应用该规则的产品Key。|
|RuleDesc|String|规则的描述信息。|
|Select|String|该规则SQL语句中的Select内容。|
|ShortTopic|String|应用该规则的具体Topic（不包含ProductKey类目），格式为：`${deviceName}/ topicShortName`。其中，`${deviceName}`指具体设备的名称，`topicShortName`是该设备的自定义类目。|
|Status|String| 该规则的运行状态。取值：

 RUNNING：运行中

 STOP：停止

 |
|Topic|String|应用该规则的具体Topic，格式为：`${productKey}/${deviceName}/ topicShortName`。|
|Where|String|该规则SQL语句中的Where查询条件。|

## 示例 {#section_ygv_n41_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetRule
&RuleId=1000
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "A8F48485-44B9-40D8-A56D-F716F384F387",
    "RuleInfo": {
        "CreateUserId": 32164***,
        "Created": "Sun Apr 08 13:42:50 CST 2018",
        "DataType": "JSON",
        "Id": 1000,
        "Modified": "Sun Apr 08 13:42:50 CST 2018",
        "Name": "rule_test_pop123",
        "ProductKey": "a1GMr***",
        "RuleDesc": "创建规则 测试",
        "Select": "a,b,c",
        "ShortTopic": "test_topic/get",
        "Status": "STOP",
        "Topic": "/a1GMr***/test_topic/get",
        "Where": "a>1"
    },
    "success": true
}
```

