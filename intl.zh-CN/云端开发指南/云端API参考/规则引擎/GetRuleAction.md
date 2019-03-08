# GetRuleAction {#reference_hvp_vjd_xdb .reference}

调用该接口查询指定规则动作的详细信息。

## 请求参数 {#section_qq3_jp1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetRuleAction。|
|ActionId|Long|是|要查询的规则动作ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_dy1_gq1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|RuleActionInfo|RuleActionInfo|调用成功时，返回的规则动作详细信息。详情参见[RuleActionInfo](#table_nct_lq1_ydb)。|

|名称|类型|描述|
|:-|:-|:-|
|Id|Long|规则动作ID。|
|RuleId|Long|该规则动作对应的规则ID。|
|Type|String| 规则动作类，取值：

 REPUBLISH：转发到另一个topic。

 OTS：存储到表格存储。

 MNS：发送消息到消息服务。

 FC：发送数据到函数计算。

 RDS：存储数据到云数据库中。

 |
|Configuration|String|该规则动作的配置信息。|
|ErrorActionFlag|String|该规则动作是否为转发错误操作数据的转发动作，即转发流转到其他云产品失败且重试失败的数据。-   true：该规则动作转发错误操作数据。
-   false：该规则动作不转发错误操作数据，而是正常转发操作。

|

## 示例 {#section_dwh_vq1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetRuleAction
&ActionId=10001
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RuleActionInfo": {
        "Type": "REPUBLISH", 
        "RuleId": 152323, 
        "Id": 100001, 
        "Configuration": "{\"topic\":\"/sys/a1zSA28HUyy/device/thing/service/property/set\",\"topicType\":0,\"uid\":\"1231579*******\"}", 
        "ErrorActionFlag": false
      }, 
      "RequestId": "F2D0755D-F350-40FE-9A6D-491859DB5E5F", 
      "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <GetRuleActionResponse>
        <RuleActionInfo>
            <Type>REPUBLISH</Type>
            <RuleId>152323</RuleId>
            <Id>100001</Id>
            <Configuration>
                <topic>/sys/a1zSA28HUyy/device/thing/service/property/set</topic>
                <topicType>0</topicType>
                <uid>1231579*******</uid>
            </Configuration>
            <ErrorActionFlag>false</ErrorActionFlag>
        </RuleActionInfo>
        <RequestId>F2D0755D-F350-40FE-9A6D-491859DB5E5F</RequestId>
        <Success>true</Success>
    </GetRuleActionResponse>
    ```


