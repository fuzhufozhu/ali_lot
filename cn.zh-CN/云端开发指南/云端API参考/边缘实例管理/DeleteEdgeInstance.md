# DeleteEdgeInstance {#reference_878614 .reference}

调用该接口删除边缘实例。

## 限制说明 {#section_3ck_7z6_n54 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_qc0_qqx_4qk .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteEdgeInstance。|
|InstanceId|String|是|边缘实例ID。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_kqu_lhy_qqe .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_g4u_zj2_cns .section}

请求示例

``` {#codeblock_l2s_sbl_88m}
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteEdgeInstance
&InstanceId=3P0TbYAz5tKNAUSF****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_x53_mut_7jg}
    {
      "RequestId": "F29C33E5-BF22-48FE-9FEF-8A24146E3388",
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_r6a_04d_87i}
    <?xml version="1.0" encoding="UTF-8"?>
    <DeleteEdgeInstanceResponse>
        <RequestId>F29C33E5-BF22-48FE-9FEF-8A24146E3388</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </DeleteEdgeInstanceResponse>
    ```


