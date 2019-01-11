# QueryBatchRegisterDeviceStatus {#reference_apr_hrz_wdb .reference}

调用该接口查询批量注册设备申请的处理状态和结果。

## 请求参数 {#section_xql_vgn_xdb .section}

|名称|类型|是否必须|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryBatchRegisterDeviceStatus。|
|ProductKey|String|是|要查询的设备所隶属的产品Key。|
|ApplyId|Long|是|要查询的申请批次ID。申请批次ID在成功调用BatchRegisterDeviceWithApplyId或BatchRegisterDevice接口的返回结果中。|

## 返回参数 {#section_yfc_qhn_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的状态信息。详情参见[DeviceApplyStatusInfo](#table_dy2_whn_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|Status|String| 申请单的处理状态和结果，取值：

 CHECK：校验。

 CHECK\_SUCCESS：校验成功。

 CHECK\_FAILED：校验失败。

 CREATE：创建。

 CREATE\_SUCCESS：创建成功。

 CREATE\_FAILED：创建失败。

 |
|ValidList|List| -   当返回Status参数值为CHECK\_FAILED或CREATE\_FAILED时，返回创建成功的设备名称集合。
-   当返回Status参数值为CHECK\_SUCCESS或CREATE\_SUCCESS时，该参数为空数组。

 |
|InvalidList|List| -   当返回Status参数值为CHECK\_FAILED或CREATE\_FAILED时，创建失败的设备名称集合。
-   当返回Status参数值为CHECK\_SUCCESS或CREATE\_SUCCESS时，该参数为空数组。

 |

## 示例 {#section_ahw_23n_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryBatchRegisterDeviceStatus
&ProductKey=al*********
&ApplyId=4416
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "Status": "CREATE_SUCCESS",
        "ValidList": {
          "Name": [
            
          ]
        },
        "InvalidList": {
          "Name": [
            
          ]
        }
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryBatchCheckDeviceNamesStatusResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <Status>CREATE_SUCCESS</Status>
            <ValidList/>
            <InvalidList/>
        </Data>
    </QueryBatchCheckDeviceNamesStatusResponse>
    ```


