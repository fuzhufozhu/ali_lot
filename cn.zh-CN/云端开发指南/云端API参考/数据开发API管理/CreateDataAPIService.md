# CreateDataAPIService {#concept_592986 .concept}

调用该接口创建数据算法服务API。

## 请求参数 {#section_mvh_g6f_ivi .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值CreateDataAPIService。|
|ApiPath|String|是| API调用地址的自定义部分。作为API资源标识符，需具有全局唯一性。

 **说明：** API调用地址的前一段部分由系统生成。

 |
|DisplayName|String|是| API的显示名称，需具有全局唯一性。仅支持中文汉字、英文字母、数字、下划线（\_）、连接符（-）、英文括号（\(\)）和空格，长度不超过20个字符。

 |
|FolderId|String|否| 保存API的文件夹。

 可在控制台[数据开发](https://iot.console.aliyun.com/la/data/develop/list)页左侧导航栏，单击**API服务**右侧的**+**号，新建文件夹。

 若不传入，默认文件夹为API列表。

 |
|Desc|String|否|API的描述。|
|OriginSql|String|是| API对应的原始SQL，指定数据开发的SQL样式。

 例如`select count(*) as deviceCount from ${system.device} where status = 1`。其中，$\{system.device\}是平台系统的设备表，具体请参见[开发任务](../../../../cn.zh-CN/数据开发/数据开发/开发任务.md#)中的表管理。

 |
|TemplateSql|String|是| 服务的模板SQL，即原始SQL的模板化。

 例如`select count(*) as deviceCount from ${system.device} where status = ${status}`。其中，$\{status\}是模板化的参数。支持设置模板参数为动态值。

 |
|RequestParams|List<RequestParam\>|否| 调用API的请求参数列表。

 基于TemplateSql中的模板参数，配置相关的请求参数。例如，针对以上模板中的$\{status\}，配置设备状态参数。

 详情参见下表RequestParam。

 |
|ResponseParams|List<ResponseParam\>|否| API的响应参数列表。

 基于TemplateSql中的模板参数，配置select字段相关的出参。例如，针对以上模板中的`select count(*) as deviceCount`，配置设备数量相关出参。

 详情参见下表ResponseParam。

 |
|公共请求参数|-|是|详情请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Name|String|是| 参数名称。

 例如，$\{status\}格式的模板参数，参数名称就是status。

 |
|Type|String|是| 参数类型，请参见[JDBCType](https://docs.oracle.com/javase/8/docs/api/java/sql/JDBCType.html)。目前，仅支持：ARRAY、VARCHAR、INTEGER、BIGINT、BOOLEAN、DECIMAL、TIMESTAMP。

 |
|Desc|String|否|参数描述。|
|Example|String|否|参数值示例。|
|Required|Boolean|否| 该参数是否必填。

 -   true：必填。
-   false：非必填。

 默认值为true。

 |

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Name|String|是| 参数名称。

 |
|Type|String|是| 参数类型，请参见[JDBCType](https://docs.oracle.com/javase/8/docs/api/java/sql/JDBCType.html)。目前，仅支持：VARCHAR、INTEGER、BIGINT、BOOLEAN、DECIMAL、TIMESTAMP。

 |
|Desc|String|否|参数描述。|
|Example|String|否|参数值示例。|

## 返回参数 {#section_1pr_8d0_hmo .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](cn.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回注册的设备信息。详情参见下表Data。|

|参数|类型|描述|
|:-|:-|:-|
|ApiSrn|String|API资源标识符，API的全局唯一标识。 示例：

 ``` {#codeblock_i6q_nhh_9r2}
acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2
```

 以上示例中，

 -   `127103983461****`是阿里云主账号ID。
-   `/device/getDeviceCountByStatus`是请求参数ApiPath的值，即API调用地址的自定义部分。

 |
|CreateTime|Long|API的创建时间。|
|LastUpdateTime|Long|API的最后更新时间。|

## 示例 {#section_lp7_f3g_q6k .section}

**请求示例**

``` {#codeblock_tjr_orf_3n9}
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateDataAPIService
&ApiPath=%2Fdevice%2FgetDeviceCountByStatus
&DisplayName=%E6%A0%B9%E6%8D%AE%E7%8A%B6%E6%80%81%E6%9F%A5%E8%AF%A2%E8%AE%BE%E5%A4%87%E6%80%BB%E6%95%B0%E6%8E%A5%E5%8F%A3
&OriginSql=SELECT%20COUNT%28%2A%29%20FROM%20%24%7Bsystem.device%7D%20WHERE%20status%20%3D%201
&RequestParam.1.Desc=%E8%AE%BE%E5%A4%87%E7%8A%B6%E6%80%81
&RequestParam.1.Example=0%EF%BC%9A%E6%9C%AA%E6%BF%80%E6%B4%BB%EF%BC%8C1%EF%BC%9A%E5%9C%A8%E7%BA%BF%EF%BC%8C3%EF%BC%9A%E7%A6%BB%E7%BA%BF%EF%BC%8C%208%EF%BC%9A%E5%B7%B2%E7%A6%81%E6%AD%A2
&RequestParam.1.Name=status
&RequestParam.1.Required=true
&RequestParam.1.Type=VARCHAR
&ResponseParam.1.Desc=%E8%AE%BE%E5%A4%87%E6%95%B0
&ResponseParam.1.Example=100
&ResponseParam.1.Name=deviceCount
&ResponseParam.1.Required=true
&ResponseParam.1.Type=INTEGER
&TemplateSql=SELECT%20COUNT%28%2A%29%20as%20deviceCount%20FROM%20%24%7Bsystem.device%7D%20WHERE%20status%20%3D%20%24%7Bstatus%7D&Timestamp=2019-06-10T10%3A00%3A05Z
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_uig_re7_sbq}
    {
        "RequestId": "57b144cf-09fc-4916-a272-a62902d5b207", 
        "Success": true, 
        "Data": {
            "ApiSrn": "acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2", 
            "CreateTime": 1557839468865, 
            "LastUpdateTime": 1557839468865
        }
    }
    ```

-   XML格式

    ``` {#codeblock_k5x_srv_6c4}
    <?xml version="1.0" encoding="UTF-8" ?>
    <CreateDataAPIServiceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <ApiSrn>acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2</ApiSrn>
            <CreateTime>1557839468865</CreateTime>
            <LastUpdateTime>1557839468865</LastUpdateTime>
        </Data>
    </CreateDataAPIServiceResponse>
    ```


