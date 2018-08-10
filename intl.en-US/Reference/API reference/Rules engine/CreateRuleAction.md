# CreateRuleAction {#reference_qzh_sjd_xdb .reference}

Call the CreateRuleAction API to create a rule action for a specified rule, to define that the processed topic messages are forwarded to other supported Alibaba Cloud services.

## Limits {#section_uzf_3fz_xdb .section}

After you call the CreateRule API to create a rule, you can use this API to create specific rule actions for the rule.

A maximum of 10 rule actions can be created for a rule.

Alibaba Cloud services that support topic data forwarding include Message Service, Function Compute, Table Store, and Message Queue.

**Note:** If you want to forward messages to High-performance Time Series DataBase \(HiTSDB\) and ApsaraDB for RDS, you define the rule actions in the IoT Platform console.

## Request parameters {#section_fjp_hfz_xdb .section}

|Name|Type|Required or Not|Description|
|----|----|---------------|-----------|
|Action|String |Yes |Action that you want to take. Set the value to CreateRuleAction.|
|RuleId|Long |Yes|ID of the rule for which you want to create an action.|
|Type|String|Yes| Type of the rule action. The value can be:

 ONS: forwards the data in the topics that have been processed by the rule engine to Alibaba Cloud Message Queue for messages distribution.

 MNS: transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Message Service for message transmission.

 FC: transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Function Compute for event computing.

 OTS: transmits the data in the topics that have been processed by the rule engine to Alibaba Cloud Table Store for NoSQL data storage.

 REPUBLISH: forwards the data in the topics that have been processed by the rule engine to another IoT topic.

 **Note:** 

-   The FC type is unavailable in Singapore, where the endpoint is ap-southeast-1. 
-   The DATAHUB, FC, and ONS types are unavailable in US \(Silicon Valley\), where the endpoint is us-west-1.
-   The OTS type does not support forwarding of binary data \(the DataType of the data is set to BINARY\).

 |
|Configuration|String |Yes |Rule action configurations. The configurations for different rule action types are different. The following section describes their respective configurations.|

##  Configuration of the REPUBLISH type {#section_ev2_m5z_xdb .section}

|Name|Description|
|:---|:----------|
|Topic|Target topic to which you forward data.|

Configuration example of the REPUBLISH type

```
{"topic":"/3dft***/bike/get"} 
```

## Configuration of the OTS type {#section_clk_3b1_ydb .section}

|Name|Description|
|:---|:----------|
|InstanceName|Name of the instance that is used to receive information in Table Store.|
|TableName|Name of the data table that is used to receive information in Table Store.|
|RegionName| Region where the target instance is located.

 The following regions are available in China \(Shanghai\), where the endpoint is cn-shanghai:

 -   cn-qingdao: China \(Qingdao\)

-   cn-beijing: China \(Beijing\)

-   cn-zhangjiakou: China \(Zhangjiakou-Beijing Winter Olympics\)

-   cn-huhehaote: China \(Hohhot\)

-   cn-hangzhou: China \(Hangzhou\)

-   cn-shanghai: China \(Shanghai\)

-   cn-shenzhen: China \(Shenzhen\)

-   cn-hongkong: Hong Kong


 The following region is available in Singapore, where the endpoint is ap-southeast-1:

 ap-southeast-1: Singapore

 The following region is available in US \(Silicon Valley\), where the endpoint is us-west-1:

 us-west-1: US \(Silicon Valley\)

 |
|Role| Information about the authorized role. You can assign a system service role to IoT Platform to authorize its access to your Table Store. The information about the authorized role is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingotsrole",
"roleName": "AliyunIOTAccessingOTSRole"
}
```

 Replace `6541***` with your Alibaba Cloud account ID. You can log on to the IoT Platform console and view your ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page.

 `AliyunIOTAccessingOTSRole` is a service role specified in RAM. The role is assigned to IoT Platform for Table Store access. For more information about this role, see the [Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

 |
|PrimaryKeys|List of primary keys in the target table. For details, see [PrimaryKeys](#table_qfn_hd1_ydb).|

|Name|Description|
|:---|:----------|
|ColumnType| Type of the primary key. The value can be:

 INTEGER: integer.

 STRING: string.

 BINARY: binary.

 |
|ColumnName|Name of the primary key.|
|ColumnValue|Value of the primary key.|
|Option|Whether the primary key is an automatically incrementing column or not. The value can be AUTO\_INCREMENT or empty. The primary key is an auto-incrementing column when its type is INTEGER and the value of Option is AUTO\_INCREMENT.|

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

|Name|Description|
|:---|:----------|
|ThemeName|Name of the target theme that is used to receive information in Message Service.|
|RegionName| Region where the instance of the target Message Service is located.

 The following regions are available in China \(Shanghai\), where the endpoint is cn-shanghai:

 -   cn-qingdao: China \(Qingdao\)

-   cn-beijing: China \(Beijing\)

-   cn-hangzhou: China \(Hangzhou\)

-   cn-shanghai: China \(Shanghai\)

-   cn-shenzhen: China \(Shenzhen\)

-   cn-chengdu: China \(Chengdu\)

-   cn-hongkong: Hong Kong


 The following region is available in Singapore, where the endpoint is ap-southeast-1:

 ap-southeast-1: Singapore

 The following region is available in US \(Silicon Valley\), where the endpoint is us-west-1:

 us-west-1: US \(Silicon Valley\)

 |
|Role| Information about the authorized role. You can assign a system service role to IoT Platform to authorize its access to your Message Service. The information about the authorized role is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingmnsrole",
"roleName": "AliyunIOTAccessingMNSRole"
}
}
```

 Replace `6541***` with your Alibaba Cloud account ID. You can log on to the IoT Platform console and view your ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page.

 `AliyunIOTAccessingMNSRole` is a service role specified in the RAM console. The role is assigned to IoT Platform for Message Service access. For more information about this role, see the [Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

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

|Name|Description|
|:---|:----------|
|FunctionName|Name of the target function that is used to receive information in Function Compute.|
|ServiceName|Name of the target service that is used to receive information in Function Compute.|
|RegionName| Region where the instance of the target Function Compute is located.

 The following regions are available in China \(Shanghai\), where the endpoint is cn-shanghai:

 -   cn-beijing: China \(Beijing\)

-   cn-hangzhou: China \(Hangzhou\)

-   cn-shanghai: China \(Shanghai\)

-   cn-shenzhen: China \(Shenzhen\)

-   cn-hongkong: Hong Kong


 Function Compute access is unavailable in Singapore, where the endpoint is ap-southeast-1.

 Function Compute access is unavailable in US \(Silicon Valley\), where the endpoint is us-west-1.

 |
|Role| Authorized role information You can assign a system service role to IoT Platform to authorize its access to your Function Compute. The information about the authorized role is as follows:

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingfcrole",
"roleName": "AliyunIOTAccessingFCRole"
}
```

 Replace `6541***` with your Alibaba Cloud ID. You can log on to the IoT Platform console and view your ID on the [Security Settings](https://account.console.aliyun.com/#/secure) page.

 `AliyunIOTAccessingFCRole` is the service role that is specified in the RAM console. The role is assigned to IoT Platform for Function Compute access. For more information about this role, see the [Role Management](https://ram.console.aliyun.com/#/role/list) page in the RAM console.

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

## Configuration of the ONS type {#section_rvs_zg1_ydb .section}

**Note:** To create a rule action for forwarding messages to Message Queue, you must authorize IoT Platform to access Message Queue \(at least authorize IoT Platform to publish messages\) in the Message Queue console or by calling the SDK of Message Queue. The fixed authorization ID is 1135850448829929.

|Name|Description|
|:---|:----------|
|Topic|Target topic for receiving information in Message Queue.|
|RegionName| Region where the instance of the target Message Queue is located.

 The following regions are available in China \(Shanghai\), where the endpoint is cn-shanghai:

 -   mq-internet-access: Internet

-   cn-qingdao: China \(Qingdao\)

-   cn-beijing: China \(Beijing\)

-   cn-hangzhou: China \(Hangzhou\)

-   cn-shanghai: China \(Shanghai\)

-   cn-shenzhen: China \(Shenzhen\)

-   cn-hongkong: Hong Kong


 **Note:** A basic Message Queue instance supports data transmission on the public network or in the same region. Only a platinum instance supports inter-region data transmission.

 The following region is available in Singapore, where the endpoint is ap-southeast-1:

 ap-southeast-1: Singapore

 Message Queue access is unavailable in US \(Silicon Valley\), where the endpoint is us-west-1.

 |

Configuration example of the ONS type

```
{
    "topic": "iotx_test_mq",
    "regionName": "cn-shanghai"
}
```

## Response parameters {#section_shd_rj1_ydb .section}

|Name|Type|Description|
|:---|:---|:----------|
|RequestId|String|Unique identifier generated by Alibaba Cloud for the request.|
|Success|Boolean|Indicates whether the call is successful. true indicates a successful call and false indicates a failed call.|
|ErrorMessage|String|The error message returned when the call fails.|
|ActionId|Long| Rule action ID generated by the rule engine for an action when the call succeeds. It is the identifier of the action.

 **Note:** Secure the information for future reference. When calling APIs related to rule actions, you may require the corresponding rule action IDs.

 |

## Examples {#section_ncq_dk1_ydb .section}

**Request example**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateRuleAction
&RuleId=1000
&Type=REPUBLISH
&Configuration={"topic":"/a1********/device1/get"}
&Common request parameters
```

**Response example**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "ActionId": 10003,
    "Success": true
}
```

