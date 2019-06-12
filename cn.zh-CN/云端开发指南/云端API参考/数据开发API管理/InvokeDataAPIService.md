# InvokeDataAPIService {#concept_592990 .concept}

调用该接口调用数据算法服务API，获取SQL查询结果。

## 请求参数 {#section_6t8_81b_n1o .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值InvokeDataAPIService。|
|ApiSrn|String|是|API资源标识符，API的全局唯一标识。 调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)成功创建API，返回的ApiSrn值。 示例：`acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2`。其中，

 -   `127103983461****`是阿里云主账号ID。
-   `/device/getDeviceCountByStatus`是请求参数ApiPath的值，即API调用地址的自定义部分。

 |
|Params|List<Param\>|是|调用API的请求参数，需根据您调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)创建API时，定义的RequestParam传入请求参数。详情请参见下表Param。|

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|ParamName|String|是|调用API的入参参数名称。必须与调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)创建API时，RequestParam中定义的Name保持一致。|
|ParamValue|String|否|调用API的入参参数值。 **说明：** 

-   统一使用String类型存储，物联网平台会根据创建API时定义的ParamType转换成JDBC类型对象。
-   创建API时，如果API请求参数类型Type定义为ARRAY类型，则不传入该参数，而需传入ListParamType和ListParamValue。

 |
|ListParamValues|List<String\>|否|ARRAY类型的参数值列表。 **说明：** 统一使用String类型存储，平台会跟据ListParamType对应的值转换成JDBC类型对象。

 |
|ListParamType|String|否|ARRAY类型的参数值的数据类型。请参见[JDBCType](https://docs.oracle.com/javase/8/docs/api/java/sql/JDBCType.html)。 目前，仅支持VARCHAR、INTEGER、BIGINT、BOOLEAN、DECIMAL、TIMESTAMP。

 |

## 返回参数 {#section_0p3_zkn_sg7 .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](cn.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回注册的设备信息。详情参见下表Data。|

|参数|类型|描述|
|:-|:-|:-|
|ApiSrn|String|API资源标识符，API的全局唯一标识。 示例：`acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2`。其中，

 -   `127103983461****`是阿里云主账号ID。
-   `/device/getDeviceCountByStatus`是请求参数ApiPath的值，即API调用地址的自定义部分。

 |
|PageNo|Integer|显示的查询结果的页码。分页码从0开始，默认为0。 如果您要自定义显示结果页，建议您在请求入参Params中增加自定义参数，如pageNo。

 |
|PageSize|Integer|每页显示的查询结果记录数。 如果您要自定义每页显示的记录数，建议您在请求入参Params中增加自定义参数，如pageSize。

 |
|FieldNameList|List<String\>|结果字段列表。 列表元素即调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)创建API时，ResponseParam中的Name定义的参数名称。|
|ResultList|List<Map<String, Object\>\>|返回的SQL处理结果。根据调用[CreateDataAPIService](cn.zh-CN/云端开发指南/云端API参考/数据开发API管理/CreateDataAPIService.md#)创建API时，ResponseParam中的Name参数，返回处理结果。 列表元素Map<String, Object\>中，

 -   key是String类型，是Name定义的参数名称；
-   Object是参数对应的值，其数据类型与ResponseParam中的Type一致。

 |

## 示例 {#section_j5q_468_wdg .section}

**请求示例**

``` {#codeblock_m50_sy9_mci}
https://iot.cn-shanghai.aliyuncs.com/?Action=InvokeDataAPIService
&ApiSrn=acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2
&Param.1.ParamName=status
&Param.1.ParamValue=1
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_2or_akp_fsf}
    {
      "Data": {
        "ResultList": {
          "ResultList": [{
              "deviceCount": 47
            }]
        },
        "PageSize": 1,
        "PageNo": 0,
        "ApiSrn": "acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2",
        "FieldNameList": {
          "FieldNameList": ["deviceCount"]
        }
      },
      "RequestId": "E68FE5DC-4D7B-4987-B785-DF8C6F191F5D",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_dlx_t80_5ed}
    <?xml version="1.0" encoding="UTF-8" ?>
    <InvokeDataAPIServiceResponse>
        <Data>
            <ResultList>
                <ResultList>
                    <deviceCount>47</deviceCount>
                </ResultList>
            </ResultList>
            <PageSize>1</PageSize>
            <PageNo>0</PageNo>
            <ApiSrn>acs:iot:*:127103983461****:serveapi/device/getDeviceCountByStatus2</ApiSrn>
            <FieldNameList>
                <FieldNameList>deviceCount</FieldNameList>
            </FieldNameList>
        </Data>
        <RequestId>E68FE5DC-4D7B-4987-B785-DF8C6F191F5D</RequestId>
        <Success>true</Success>
    </InvokeDataAPIServiceResponse>
    ```


