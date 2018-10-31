# ListRule {#reference_uzn_wjd_xdb .reference}

调用该接口分页查询所有规则列表。

## 请求参数 {#section_mqh_jr1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：ListRule。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值100，默认值是10。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。最大取值 1000，默认值 1。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_slq_tr1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|RuleInfo|调用成功时，返回的规则信息列表。详情参见[RuleInfo](#table_lbq_bs1_ydb)。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页码。|
|Total|Integer|总页数。|

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
|ShortTopic|String|应用该规则的具体Topic（不包含ProductKey类目），格式为：`${deviceName}/ topicShortName`。其中，$\{deviceName\}指具体设备的名称，topicShortName是该设备的自定义类目。|
|Status|String| 该规则的运行状态。取值：

 RUNNING：运行中

 STOP：停止

 |
|Topic|String|应用该规则的具体Topic，格式为：`${productKey}/${deviceName}/ topicShortName`。|
|Where|String|该规则SQL语句中的Where查询条件。|

## 示例 {#section_wx2_4s1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=ListRule
&PageSize=10
&CurrentPage=1
&公共请求参数
```

**返回示例**

```
{
    "Data": [{
        "CreateUserId": 32164***,
        "Created": "Sun Apr 08 13:42:50 CST 2018",
        "DataType": "JSON",
        "Id": 1000,
        "Modified": "Sun Apr 08 13:42:50 CST 2018",
        "Name": "rule_test_pop123",
        "ProductKey": "HMyBnl***",
        "RuleDesc": "创建规则测试",
        "Select": "a,b,c",
        "ShortTopic": "test_topic/get",
        "Status": "STOP",
        "Topic": "/HMyBnl***/test_topic/get",
        "Where": "a>1"
    },
    {
        "CreateUserId": 32164***,
        "Created": "Sun Apr 08 11:29:05 CST 2018",
        "DataType": "BINARY",
        "Id": 1001,
        "Modified": "Sun Apr 08 11:33:05 CST 2018",
        "Name": "创建规则 测试",
        "ProductKey": "HMyBnl***",
        "Select": "*",
        "ShortTopic": "test_topic/get",
        "Status": "RUNNING",
        "Topic": "/HMyBnl***/test_topic/get"
    }],
    "Page": 1,
    "PageSize": 2,
    "RequestId": "43ED5445-90B1-4A97-95A1-8BF6F48CD6E9",
    "Success": true,
    "Total": 30
}
```

