# CreateRuleAction {#reference_qzh_sjd_xdb .reference}

Call this operation to create a rule action for a specified rule to forward processed topic data to another topic or to another supported Alibaba Cloud services.

## Limits {#section_uzf_3fz_xdb .section}

For information about the regions and supported target products, see [Regions and zones](../../../../reseller.en-US/User Guide/Rules engine/Regions and zones.md#).

After you call the CreateRule interface to create a rule, you can call this interface to create specific rule actions for the rule and define message forwarding targets.

You can create up to ten rule actions for a rule.

Using this API, you can define actions to forward topic messages to Message Service, Function Compute, and Table Store.

**Note:** If you want to forward messages to ApsaraDB for RDS, you define the rule actions in the IoT Platform console.

## Request parameters {#section_fjp_hfz_xdb .section}

|Parameter|Type|Required|Description|
|:--------|:---|:-------|:----------|
|Action|String|Yes|The action that you want to perform. Set the value to CreateRuleAction.|
|RuleId|Long|Yes|The ID of the rule for which you want to create an action.|
|Type|String|Yes| The rule action type indicates the target to which data will be forwarded. Value options:

 MNS : Forwards data to a theme of Message Service \(MNS\) for message transmission.

 FC : Forwards data to a function of Function Compute for event computing.

 OTS: Forwards data to a Table Store instance for NoSQL data storage.

 REPUBLISH: Forwards data to another topic.

 |
|Configuration|String|Yes|The configurations of the rule action. The configurations for different rule action types are different. The following section describes their respective configurations.|
|Common Request Parameters|-|Yes|See [Common parameters](reseller.en-US/Developer Guide (Cloud)/API reference/Common parameters.md#).|

## Configuration of the REPUBLISH type {#section_ev2_m5z_xdb .section}

|Parameter|Description|
|:--------|:----------|
|topic| The target topic \(a system-defined topic or a custom topic\).

 Supported system-defined topics for Pro Edition devices are as follows:

-   `/sys/${YourProductKey}/${YourDeviceName}/thing/service/property/set`
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/service/${tsl.service.identifier}`. the variable value of $\{tsl.service.identifier\} is from the TSL of the device product.

 |
|topicType|The topic type.-   0: Indicates the topic is a system-defined topic of a Pro Edition product.
-   1: Indicates the topic is a custom topic.

|

Configuration example of the REPUBLISH type

```
A system-defined topic: {"topic":"/sys/a1TXXXXXWSN/xxx_cache001/thing/service/property/set","topicType":0}
A custom topic: {"topic":"/a1TXXXXXWSN/xxx_cache001/user/update","topicType":1}
```

## Configuration of the OTS type {#section_clk_3b1_ydb .section}

|Parameter|Description|
|:--------|:----------|
|instanceName|The name of the Table Store instance that is used to receive data from IoT Platform.|
|tableName|The name of the data table that is used to receive data from IoT Platform.|
|regionName| The region ID where the target instance is located.

 See [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm) for region IDs.

 |
|role| The information of the role to be authorized to IoT Platform. You assign a system service role to IoT Platform to authorize its access to your Table Store instance. An example of the authorized role information is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingotsrole",
"roleName": "AliyunIOTAccessingOTSRole"
}
```

 Replace `6541***` with your account ID. Log on to the console, and you can see your account ID on the [Account Management](https://partners-intl.console.aliyun.com) page.

 `AliyunIOTAccessingOTSRole` is a service role specified in RAM. The role is assigned to IoT Platform for Table Store access. Log on to the RAM console, and on the page of [Role Management](https://partners-intl.console.aliyun.com/?spm=a2c63.p38356.a3.3.13262fc43XQGoZ#/ram), you can manage your roles.

 |
|primaryKeys|Primary keys in the target table. For more information, see [PrimaryKeys](#table_qfn_hd1_ydb).|

|Parameter|Description|
|:--------|:----------|
|columnType| The type of the primary key. Value options:

 INTEGER: Integer.

 STRING: String.

 BINARY: Binary.

 |
|columnName|The name of the primary key.|
|columnValue|The primary key value.|
|option|Whether the primary key is an automatically incrementing column or not. The value can be AUTO\_INCREMENT or empty. When the columnType is INTEGER and the value of option is AUTO\_INCREMENT, The primary key is an auto-incrementing column.|

Configuration example of the OTS type

```
{
    "instanceName": "testaaa",
    "tableName": "tt",
    "primaryKeys": [
        {
            "columnType": "STRING",
            "columnName": "ttt",
            "columnValue": "${tt}",
            "option": ""
        },
        {
            "columnType": "INTEGER",
            "columnName": "id",
            "columnValue": "",
            "option": "AUTO_INCREMENT"
        }
    ],
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingotsrole",
        "roleName": "AliyunIOTAccessingOTSRole"
    }
}
```

## Configuration of the MNS type {#section_l5r_f21_ydb .section}

|Parameter|Description|
|:--------|:----------|
|themeName|The name of the target Message Service theme that is used to receive data from IoT Platform.|
|regionName| The region ID where the target Message Service theme is located.

 See [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm) for region IDs.

 |
|role| The information of the role to be authorized to IoT Platform. You assign a system service role to IoT Platform to authorize its access to your Message Service. An example of the authorized role information is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingmnsrole",
"roleName": "AliyunIOTAccessingMNSRole"
}
}
```

 Replace `6541***` with your account ID. Log on to the console, and you can see your account ID on the [Account Management](https://partners-intl.console.aliyun.com) page.

 `AliyunIOTAccessingMNSRole` is a service role specified in RAM. The role is assigned to IoT Platform for Message Service access. Log on to the RAM console, and on the page of [Role Management](https://partners-intl.console.aliyun.com/?spm=a2c63.p38356.a3.3.13262fc43XQGoZ#/ram), you can manage your roles.

 |

Configuration​​ example of the MNS type

```
{
    "themeName": "mns-test-topic1",
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingmnsrole",
        "roleName": "AliyunIOTAccessingMNSRole"
    }
}
```

## Configuration of the FC type {#section_ozz_nf1_ydb .section}

|Parameter|Description|
|:--------|:----------|
|functionName|The name of the target function in Function Compute that is used to receive data from IoT Platform.|
|serviceName|The name of the service in Function Compute.|
|regionName| The region ID where the target function of Function Compute is located.

 See [Regions and zones](https://partners-intl.aliyun.com/help/doc-detail/40654.htm) for region IDs.

 |
|role| The information of the role to be authorized to IoT Platform. You assign a system service role to IoT Platform to authorize its access to your Function Compute. An example of the authorized role information is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingfcrole",
"roleName": "AliyunIOTAccessingFCRole"
}
```

 Replace `6541***` with your account ID. Log on to the console, and you can see your account ID on the [Account Management](https://partners-intl.console.aliyun.com) page.

 `AliyunIOTAccessingFCRole` is a service role specified in RAM. The role is assigned to IoT Platform for Function Compute access. Log on to the RAM console, and on the page of [Role Management](https://partners-intl.console.aliyun.com/?spm=a2c63.p38356.a3.3.13262fc43XQGoZ#/ram), you can manage your roles.

 |

Configuration example of the FC type

```
{
    "regionName": "cn-shanghai",
    "role": {
        "roleArn": "acs:ram::5645***:role/aliyuniotaccessingfcrole",
        "roleName": "AliyunIOTAccessingFCRole"
    },
    "functionName": "weatherForecast",
    "serviceName": "weather"
}
```

## Response Parameters {#section_shd_rj1_ydb .section}

|Parameter|Type|Description|
|:--------|:---|:----------|
|RequestId|String|The globally unique ID generated by Alibaba Cloud for the request.|
|Success|Boolean|Indicates whether the call is successful. A value of true indicates that the call is successful. A value of false indicates that the call has failed.|
|ErrorMessage|String|The error message returned when the call fails.|
|Code|String|The error code returned when the call fails. For more information about error codes, see [Error codes](reseller.en-US/Developer Guide (Cloud)/API reference/Error codes.md#).|
|ActionId|Long| Rule action ID generated by Rules Engine for an action when the call succeeds. It is the identifier of the action.

 **Note:** Save the action ID for future reference. When you call APIs related to this rule action, you are required to provide the ID.

 |

## Examples {#section_ncq_dk1_ydb .section}

**Request example**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateRuleAction
&RuleId=1000
&Type=REPUBLISH
&Configuration={"topic":"/a1********/device1/get","topicType":1}
&Public Request Parameters
```

**Response example**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "ActionId": 10003,
    "Success": true
}
```

