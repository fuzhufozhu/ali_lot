# GetDataAPIServiceDetail {#concept_592987 .concept}

调用该接口获取数据算法服务API详情。

## 请求参数 {#section_i5e_kd0_rn4 .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：GetDataAPIServiceDetail。|
|ApiSrn|String|是|API资源标识符，API的全局唯一标识。 调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)成功创建API，返回的ApiSrn值。 格式：``

 ``` {#codeblock_391_wf7_y8j}
acs:iot:*:${aliyunuserID}:serveapi/${ApiPath}
```

 示例：

 ``` {#codeblock_fum_7gm_5ux}
acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2
```

 以上示例中，

 -   `127103983461****`是阿里云主账号ID。
-   `/device/getDeviceCountByStatus`是API调用地址的自定义部分。

 |
|公共请求参数|-|是|详情参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_slf_7q9_rea .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](cn.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情参见下表Data。|

|参数|类型|描述|
|:-|:-|:-|
|ApiSrn|String|API资源标识符，API的全局唯一标识。|
|Status|Integer| API的状态。

 -   0：可编辑。
-   1：已测试。
-   2：已发布。

 |
|DisplayName|String| API名称。

 |
|ApiPath|String| API调用地址的自定义部分。

 |
|CreateTime|Long| API的创建时间。

 |
|LastUpdateTime|Long| API的最后更新时间。

 |
|Description|String| API的描述信息。

 |
|SqlTemplateDTO|String| SQL模板信息。

 调用成功时，返回的SQL模板数据。详情参见下表SqlTemplateDTO。

 |

|参数|类型|描述|
|:-|:-|:-|
|OriginSql|String| API对应的原始SQL。

 |
|TemplateSql|String| 原始SQL的模板化SQL。

 |
|RequestParams|List<RequestParam\>| 调用API的请求参数列表。

 详情参见下表RequestParam。

 |
|ResponseParams|List<ResponseParam\>| API的响应参数列表。

 详情参见下表ResponseParam。

 |

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Name|String|是| 请求参数名称。

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
|Name|String|是| 返回参数名称。

 |
|Type|String|是| 参数类型，请参见[JDBCType](https://docs.oracle.com/javase/8/docs/api/java/sql/JDBCType.html)。目前，仅支持：VARCHAR、INTEGER、BIGINT、BOOLEAN、DECIMAL、TIMESTAMP。

 |
|Desc|String|否|参数描述。|
|Example|String|否|参数值示例。|

## 示例 {#section_zcl_5w2_dte .section}

**请求示例**

``` {#codeblock_2k0_in4_ahl}
https://iot.cn-shanghai.aliyuncs.com/?Action=GetDataAPIServiceDetail
&ApiSrn=acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_k6b_tt2_41u}
    {
       "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
       "Success":true,
       "Data":{
          "ApiSrn":"acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2",
          "CreateTime":1557839468865,
          "LastUpdateTime":1557839468865,
          "Status": 1,
          "DisplayName":"根据状态获取设备数",
          "ApiPath":"/device/getDeviceCountByStatus",
          "Description":"描述",
          "SqlTemplateDTO":{
             "OriginSql":"SELECT COUNT(*) FROM ${system.device} WHERE status = 1",
             "TemplateSql":"SELECT COUNT(*) as deviceCount FROM ${system.device} WHERE status = ${status}",
             "RequestParams":[
                {
                   "name":"status",
                   "type":"INTEGER",
                   "desc":"设备状态",
                   "example":"0",
                   "required":true
                }
             ],
             "ResponseParams":[
                {
                   "name":"deviceCount",
                   "type":"INTEGER",
                   "desc":"设备数",
                   "example":"100"
                }
             ]
          }
       }
    }
    ```

-   XML格式

    ``` {#codeblock_mhx_36d_mdj}
    <?xml version="1.0" encoding="UTF-8" ?>
    <GetDataAPIServiceDetailResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <ApiSrn>acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2</ApiSrn>
            <CreateTime>1557839468865</CreateTime>
            <LastUpdateTime>1557839468865</LastUpdateTime>
            <Status>1</Status>
            <DisplayName>根据状态获取设备数</DisplayName>
            <ApiPath>/device/getDeviceCountByStatus</ApiPath>
            <Description>描述</Description>
            <SqlTemplateDTO>
                <OriginSql>SELECT COUNT(*) FROM ${system.device} WHERE status = 1</OriginSql>
                <TemplateSql>SELECT COUNT(*) as deviceCount FROM ${system.device} WHERE status = ${status}</TemplateSql>
                <RequestParams>
                    <name>status</name>
                    <type>INTEGER</type>
                    <desc>设备状态</desc>
                    <example>0</example>
                    <required>true</required>
                </RequestParams>
                <ResponseParams>
                    <name>deviceCount</name>
                    <type>INTEGER</type>
                    <desc>设备数</desc>
                    <example>100</example>
                </ResponseParams>
            </SqlTemplateDTO>
        </Data>
    </GetDataAPIServiceDetailResponse>
    ```


