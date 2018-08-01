# CreateRuleAction {#reference_qzh_sjd_xdb .reference}

调用该接口在指定的规则下创建一个规则动作，定义将处理后的Topic数据转发至支持的其他阿里云服务。

## 限制说明 {#section_uzf_3fz_xdb .section}

规则引擎支持的地域及目的地云产品，从[这里](../../../../intl.zh-CN/用户指南/规则引擎/地域和可用区.md#)查询。

在调用CreateRule接口创建规则后，您可以调用本接口，在规则下创建具体的规则动作，定义将处理后的Topic数据转发至支持的其他阿里云服务。

一个规则下面最多可创建10个规则动作。

支持Topic数据转发的阿里云服务包括：消息服务、函数计算、表格存储、和消息队列。

**说明：** 如果您想对接高性能时序数据库（HiTSDB）和云数据库RDS版，建议您通过控制台操作。

## 请求参数 {#section_fjp_hfz_xdb .section}

|名称|类型|是否必需|描述|
|--|--|----|--|
|Action|String|是|要执行的操作，取值：CreateRuleAction。|
|RuleId|Long|是|要为其创建动作的规则ID。|
|Type|String|是| 规则动作类型，取值：

 ONS：将根据规则处理后的Topic数据转发至阿里云消息队列，进行消息分发。

 MNS：将根据规则处理后的Topic数据发送至阿里云消息服务（Message Service）中，进行消息传输。

 FC：将根据规则处理后的Topic数据发送至阿里云函数计算服务，进行事件计算。

 OTS：将根据规则处理后的Topic数据发送至阿里云表格存储，进行NoSQL数据存储。

 REPUBLISH：将根据规则处理后的Topic数据转发至另一个IoT Topic。

 |
|Configuration|String|是|该规则动作的配置信息。不同规则动作类型所需内容不同，具体要求见下文描述。|

## REPUBLISH类型Configuration定义 {#section_ev2_m5z_xdb .section}

|名称|描述|
|:-|:-|
|Topic| 转发至的目标Topic。

 高级版下行Topic如下：

-   `/sys/${productkey}/${devicename}/thing/service/property/set`
-   `/sys/${productkey}/${devicename}/thing/service/${tsl.service.identifier}`，$\{\}中的内容由该产品物模型中的服务决定。

 |
|topicType|Topic的类型。-   0表示高级版产品下行Topic。
-   1表示用户自定义Topic。

|

REPUBLISH 类型Configuration示例：

```
sys类型：{"topic":"/sys/a1TXXXXXWSN/xxx_cache001/thing/service/property/set","topicType":0}
自定义类型：{"topic":"/a1TXXXXXWSN/xxx_cache001/user/update","topicType":1}
```

## OTS类型Configuration定义 {#section_clk_3b1_ydb .section}

|名称|描述|
|:-|:-|
|InstanceName|表格存储中用来接收信息的实例名称。|
|TableName|表格存储中用来接收信息的数据表名称。|
|RegionName| 目标实例所在地域。

 具体RegionName写法请参考[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。

 |
|Role| 授权角色信息。通过授予IoT指定的系统服务角色，您可以授权IoT访问您的表格存储。授权角色信息如下：

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingotsrole",
"roleName": "AliyunIOTAccessingOTSRole"
}
```

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在 [账号安全设置](https://account.console.aliyun.com/#/secure) 页面查看您的账号ID。

 `AliyunIOTAccessingOTSRole`是访问控制中定义的服务角色。用于授予IoT访问表格存储。关于该角色的更多信息，您可以登录访问控制控制台，在[角色管理](https://ram.console.aliyun.com/#/role/list)页面查看。

 |
|PrimaryKeys|目标表中的主键列表。详情参见[PrimaryKey](#table_qfn_hd1_ydb)。|

|名称|描述|
|:-|:-|
|ColumnType| 主键类型，取值：

 INTEGER：整型

 STRING：字符串

 BINARY：二进制

 |
|ColumnName|主键名称。|
|ColumnValue|主键值。|
|Option|主键是否为自增列，取值：AUTO\_INCREMENT或为空。当主键类型为INTEGER，且该字段为AUTO\_INCREMENT时，主键为自增列。|

OTS类型Configuration示例：

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

## MNS类型Configuration定义 {#section_l5r_f21_ydb .section}

|名称|描述|
|:-|:-|
|ThemeName|消息服务中用来接收信息的目标主题名称。|
|RegionName| 目标消息服务所在区域。

 具体RegionName写法请参考[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。

 |
|Role| 授权角色信息。通过授予IoT指定的系统服务角色，您可以授权IoT访问您的消息服务。授权角色信息如下：

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingmnsrole",
"roleName": "AliyunIOTAccessingMNSRole"
}
}
```

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在 [账号安全设置](https://account.console.aliyun.com/#/secure) 页面查看您的账号ID。

 `AliyunIOTAccessingMNSRole`是访问控制中定义的服务角色。用于授予IoT访问消息服务。关于该角色的更多信息，您可以登录访问控制控制台，在[角色管理](https://ram.console.aliyun.com/#/role/list)页面查看。

 |

MNS类型​Configuration​​示例：

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

## FC类型Configuration定义 {#section_ozz_nf1_ydb .section}

|名称|描述|
|:-|:-|
|FunctionName|函数服务中用来接收信息的目标函数名称。|
|ServiceName|函数服务中用来接收信息的目标服务名称。|
|RegionName| 目标函数服务实例所在区域。

 具体RegionName写法请参考[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。

 |
|Role| 授权角色信息。通过授予IoT指定的系统服务角色，您可以授权IoT访问您的函数计算服务。授权角色信息如下：

 ```
{
"roleArn": "acs:ram::6541***:role/aliyuniotaccessingfcrole",
"roleName": "AliyunIOTAccessingFCRole"
}
```

 请将`6541***`替换成您的阿里云账号ID。您可以登录控制台，在 [账号安全设置](https://account.console.aliyun.com/#/secure) 页面查看您的账号ID。

 `AliyunIOTAccessingFCRole`是访问控制中定义的服务角色。用于授予IoT访问函数计算。关于该角色的更多信息，您可以登录访问控制控制台，在[角色管理](https://ram.console.aliyun.com/#/role/list)页面查看。

 |

FC类型Configuration示例

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

## ONS类型Configuration定义 {#section_rvs_zg1_ydb .section}

**说明：** 您需通过调用消息队列的SDK，或在消息队列控制台，授权IoT访问消息队列（至少要授予IoT发布权限），然后才能够成功创建将Topic数据转发至Message Queue的规则动作。固定授权账号：1135850448829929。

|名称|描述|
|:-|:-|
|Topic|消息队列中用来接收信息的目标Topic。|
|RegionName| 目标消息队列实例所在区域。

 具体RegionName写法请参考[地域和可用区](https://www.alibabacloud.com/help/doc-detail/40654.htm)文档。

 **说明：** 公网和同区流转，使用普通版消息队列实例即可；如果您需要跨区流转，则消息队列实例必需是铂金版实例。

 |

ONS类型Configuration示例

```
{
"queneName": "aliyun-iot-queue",
"regionName": "cn-hangzhou",
"role": {
"roleArn": "acs:ram::XXXXX:role/aliyuniotaccessingmnsrole",
"roleName": "AliyunIOTAccessingMNSRole",
}
}
```

## 返回参数 {#section_shd_rj1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|ActionId|Long| 调用成功时，规则引擎为该规则动作生成的规则动作ID，作为其标识符。

 **说明：** 请妥善保管该信息。在调用与规则动作相关的接口时，您可能需要提供对应的规则动作ID。

 |

## 示例 {#section_ncq_dk1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateRuleAction
&RuleId=1000
&Type=REPUBLISH
&Configuration={"topic":"/a1********/device1/get","topicType":1}
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "ActionId": 10003,
    "Success": true
}
```

