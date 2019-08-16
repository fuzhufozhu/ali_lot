# BatchUnbindDeviceFromEdgeInstance {#reference_878635 .reference}

调用该接口批量将设备从边缘实例中移除。

## 限制说明 {#section_k5p_d5w_p19 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_iua_pn8_a02 .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchUnbindDeviceFromEdgeInstance。|
|InstanceId|String|是|边缘实例ID。|
|IotIds|List\(String\)|是|要移除的设备ID列表。 可调用[QueryEdgeInstanceDevice](cn.zh-CN/云端开发指南/云端API参考/边缘实例管理/QueryEdgeInstanceDevice.md#)查询实例中的设备IotId。

 单次调用最多可移除20个设备。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_msd_992_kii .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_gq1_scm_q7x .section}

请求示例

``` {#codeblock_y2d_6bq_hia}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchUnbindDeviceFromEdgeInstance
&IotIds.2=BXPV9Ks3bxwM9fD****0000101
&IotIds.1=sjI0Sd124XFYyjBY****000101
&InstanceId=F3APY0tPLhmgGtx0****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_i8e_xsi_hb5}
    {
     "RequestId": "34755DC3-2809-4AE2-BAD8-7B81ED69D570",
     "Code": "Success",
     "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_r33_s4y_843}
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchUnbindDeviceFromEdgeInstanceResponse>
        <RequestId>34755DC3-2809-4AE2-BAD8-7B81ED69D570</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </BatchUnbindDeviceFromEdgeInstanceResponse>
    ```


