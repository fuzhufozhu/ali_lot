# UpdateEdgeInstance {#reference_878615 .reference}

调用该接口更新边缘实例。

## 限制说明 {#section_xwe_076_d04 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为5。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_764_my3_7cd .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：UpdateEdgeInstance。|
|InstanceId|String|是|边缘实例ID。|
|Name|String|是|实例名称。 支持中文汉字、英文大小写、数字、下划线（\_）和连接号（-），不超过20个字符（一个中文汉字算2个字符）。

 |
|Tags|String|否|实例标签。标签由key:value组成，多个标签用英文逗号隔开，如`k1:v1,k2:v2`。 -   标签key：
    -   不可为空。
    -   在该边缘实例中具有唯一性。
    -   仅支持英文大小写。
    -   不可超过20个字符。
-   标签value：
    -   不可为空。
    -   支持中文、英文大小写、数字、下划线（\_）和连接号（-）。
    -   不可超过20个字符（一个中文汉字算2个字符）。

 |
|Spec|Integer|否|产品规格。 -   10：轻量版。
-   20：标准版。
-   30：专业版。

 |
|BizEnable|Boolean|否|是否开启边缘实例。取值： -   true：开启。
-   false：关闭。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_57e_ni3_swu .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_8dg_83c_7oj .section}

请求示例

``` {#codeblock_uas_w15_00v}
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateEdgeInstance
&InstanceId=F3APY0tPLhmgGtx0****
&Spec=30
&Tags=room:1,floor:1
&BizEnable=true
&Name=测试实例new
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_brm_3z9_va2}
    {
      "RequestId": "10CA6DAD-EBAF-4D3E-9309-9DB5B0FF48F2",
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_vc9_1an_sik}
    <?xml version="1.0" encoding="UTF-8"?>
    <UpdateEdgeInstanceResponse>
        <RequestId>10CA6DAD-EBAF-4D3E-9309-9DB5B0FF48F2</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </UpdateEdgeInstanceResponse>
    ```


