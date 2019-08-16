# BatchBindDeviceToEdgeInstanceWithDriver {#reference_878633 .reference}

调用该接口批量将设备分配到边缘实例中，并为设备设置驱动。

## 限制说明 {#section_78a_r6j_nbh .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_xvq_61r_m8w .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchBindDeviceToEdgeInstanceWithDriver。|
|InstanceId|String|是|边缘实例ID。|
|DriverId|String|是|驱动ID。|
|IotIds|List\(String\)|是|设备ID列表。 可调用[QueryDevice](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevice.md#)查询当前账号下所有设备信息，获取设备IotId。

 单次调用最多可绑定20个设备。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_pmj_lnj_x9w .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_vmp_d39_tkj .section}

请求示例

``` {#codeblock_cei_70y_bht}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchBindDeviceToEdgeInstanceWithDriver
&IotIds.3=BXPV9Ks3bxwM9fD****0000101
&IotIds.2=sjI0Sd124XFYyjB****O000101
&IotIds.1=8n5sFqlfnchlBjct****000101
&DriverId=021d154d2a2f4dd7a489773d9e04****
&InstanceId=F3APY0tPLhmgGtx0****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_nt8_835_vme}
    {
     "RequestId": "BFFA9519-6AF1-4D15-AFAF-FD412714C1BE",
     "Code": "Success",
     "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_9jk_g72_cam}
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchBindDeviceToEdgeInstanceWithDriverResponse>
        <RequestId>BFFA9519-6AF1-4D15-AFAF-FD412714C1BE</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </BatchBindDeviceToEdgeInstanceWithDriverResponse>
    ```


