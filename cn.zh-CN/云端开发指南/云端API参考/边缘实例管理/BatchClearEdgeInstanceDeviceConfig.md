# BatchClearEdgeInstanceDeviceConfig {#reference_878642 .reference}

调用该接口批量清空边缘实例设备配置。

## 限制说明 {#section_gp5_h7r_uw6 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_e84_bxo_219 .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchClearEdgeInstanceDeviceConfig。|
|InstanceId|String|是|边缘实例ID。|
|IotIds|List\(String\)|是|要清空配置的设备ID列表。 可调用[QueryEdgeInstanceDevice](cn.zh-CN/云端开发指南/云端API参考/边缘实例管理/QueryEdgeInstanceDevice.md#)查询实例中的设备ID。

 单次调用最多可清空20个设备配置。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_3uo_omg_9mo .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_ak7_e91_vy0 .section}

请求示例

``` {#codeblock_xfi_pbf_t8d}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchClearEdgeInstanceDeviceConfig
&IotIds.2=BXPV9Ks3bxwM9fD****0000101
&IotIds.1=sjI0Sd124XFYyjBY****000101
&InstanceId=F3APY0tPLhmgGtx0****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_cyt_4c9_bqz}
    {
     "RequestId": "0BC2AA1C-E4D0-4E78-A70F-08C9A90686B0",
     "Code": "Success",
     "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_sdj_dvl_efy}
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchClearEdgeInstanceDeviceConfigResponse>
        <RequestId>0BC2AA1C-E4D0-4E78-A70F-08C9A90686B0</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </BatchClearEdgeInstanceDeviceConfigResponse>
    ```


