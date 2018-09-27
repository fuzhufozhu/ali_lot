# CreateRuleAction {#reference_qzh_sjd_xdb .reference}

Call the CreateRuleAction API to create a rule action under a specified rule and forward the processed topic data to other supported Alibaba Cloud services.

## Limits {#section_uzf_3fz_xdb .section}

Click [here](../../../../reseller.en-US/User Guide/Rules engine/Regions and zones.md#) to query the regions and Alibaba Cloud products supported by the rule engine.

After you call the CreateRule interface to create a rule, you can call this interface to create specific rule actions for the rule and define message forwarding targets.

A maximum of 10 rule actions can be created for a rule.

You can forward your topic messages to Message Service, Function Compute, and Table Store.

**Note:** If you want to forward messages to ApsaraDB for RDS, you define the rule actions in the IoT Platform console.

## Request parameters {#section_fjp_hfz_xdb .section}

|Name|Type|Required|Description|
|----|----|--------|-----------|
|Action |String | Yes|Action that you want to take. Set the value to CreateRuleAction.|
|RuleId|Long |Yes|ID of the rule for which you want to create an actions.|
|Type |String | Yes| The rule action type. Values include:

 MNS:  Transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Message Service for message transmission.

 FC: Transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Function Compute for event computing.

 OTS: Transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Table Store for NoSQL data storage.

 REPUBLISH: Forwards the data in topics that have been processed by the rule engine to another IoT topic.

 |
|Configuration|String | Yes|Rule action configurations. The configurations for different rule action types are different. The following section describes their respective configurations.|

## Configuration of the REPUBLISH type {#section_ev2_m5z_xdb .section}

|Name |Description|
|:----|:----------|
|topic| Target topic to which you forward data.

 System topics of Pro Edition products which can be used are as following:

-   `/sys/${YouProductKey}/${YouDeviceName}/thing/service/property/set`
-   `/sys/${YouProductKey}/${YouDeviceName}/thing/service/${tsl.service.identifier}`  For the variables, you enter your device information and TSL service identifier.

 |
|topicType|The topic type.-   0 indicates the system topics of Pro Edition products.
-   1 indicates the customized topics.

|

Configuration examples of the REPUBLISH type

```
A system topic: {"topic":"/sys/a1TXXXXXWSN/xxx_cache001/thing/service/property/set","topicType":0}
A customized topic: {"topic":"/a1TXXXXXWSN/xxx_cache001/user/update","topicType":1}
```

## Configuration of the OTS type {#section_clk_3b1_ydb .section}

|Name |Description|
|:----|:----------|
|instanceName|Name of the instance that is used to receive information in Table Store.|
|tableName|Name of the data table that is used to receive information in Table Store.|
|regionName| Select the region where the target instance is located.

 See [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm) for RegionName.

 |
|role| Information about the authorized role. You assign a system service role to IoT Platform to authorize its access to your Table Store. The formats of the authorized role information are as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingotsrole",
"roleName": "AliyunIOTAccessingOTSRole"
}
```

 Replace `6541***` with your Alibaba Cloud account ID. Log on to the Alibaba Cloud console, and you can see your account ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page of your account management.

 `AliyunIOTAccessingOTSRole` is a service role specified in RAM. The role is assigned to IoT Platform for Table Store access. For more information about roles, see the [Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

 |
|primaryKeys|List of primary keys in the target table.  For more information, see [PrimaryKeys](#table_qfn_hd1_ydb).|

|Name |Description|
|:----|:----------|
|columnType| Type of the primary key. The value can be:

 INTEGER: Integer.

 STRING: String.

 BINARY: Binary data.

 |
|columnName|Name of the primary key.|
|columnValue|Value of the primary key.|
|option|Whether the primary key is an automatically incrementing column or not. The value can be AUTO\_INCREMENT or empty. The primary key is an auto-incrementing column when its type is INTEGER and the value of Option is AUTO\_INCREMENT.|

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

|Name |Description|
|:----|:----------|
|themeName|Name of the target theme that is used to receive information in Message Service.|
|regionName| Region where the instance of the target Message Service is located.

 See [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm) for RegionName.

 |
|role| Information about the authorized role. You assign a system service role to IoT Platform to authorize its access to your Message Service. The formats of the authorized role information are as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingmnsrole",
"roleName": "AliyunIOTAccessingMNSRole"
}
}
```

 Replace `6541***` with your Alibaba Cloud account ID. Log on to the Alibaba Cloud console, and you can see your account ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page of your account management.

 `AliyunIOTAccessingMNSRole` is a service role specified in RAM. The role is assigned to IoT Platform for Message Service access. For more information about roles, see the[Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

 |

​Configuration​​ example of the MNS type

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

|Name |Description|
|:----|:----------|
|functionName|Name of the target function that is used to receive information in Function Compute.|
|serviceName|Name of the target service that is used to receive information in Function Compute.|
|regionName| Region where the instance of the target Function Compute is located.

 See [Regions and zones](https://www.alibabacloud.com/help/doc-detail/40654.htm) for RegionName.

 |
|role| Information about the authorized role. You assign a system service role to IoT Platform to authorize its access to your Function Compute. The formats of the authorized role information are as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingfcrole",
"roleName": "AliyunIOTAccessingFCRole"
}
```

 Replace `6541***` with your Alibaba Cloud account ID. Log on to the Alibaba Cloud console, and you can see your account ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page of your account management.

 `AliyunIOTAccessingFCRole` is a service role specified in RAM. The role is assigned to IoT Platform for Function Compute access. For more information about roles, see the[Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

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

## Response parameters {#section_shd_rj1_ydb .section}

|Name |Type|Description|
|:----|:---|:----------|
|RequestId|String|The GUID generated by Alibaba Cloud for the request.|
|Success|Boolean |Indicates whether the call is successful. A value of true indicates that the call is successful. A value of false indicates that the call has failed.|
|ErrorMessage|String|The error message returned when the call fails.|
|ActionId|Long| Rule action ID generated by the rule engine for an action when the call succeeds. It is the identifier of the action.

 **Note:** Save the information for future reference. When you call APIs related to rule actions, you may be required to enter the corresponding rule action IDs.

 |

## Examples {#section_ncq_dk1_ydb .section}

**Request example**

**Note:** For your easy understanding of the format of the parameter values, in the following example, the special characters in parameter values are not encoded. However, when you call the API in practice, the special characters must be encoded.

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateRuleAction
&RuleId=1000
&Type=REPUBLISH
&Configuration={"topic":"/a1********/device1/get","topicType":1}
&common request parameters
```

**Response example**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "ActionId": 10003,
    "Success": true
}
```

