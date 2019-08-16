# CreateEdgeInstance {#reference_878612 .reference}

调用该接口创建边缘实例。

## 限制说明 {#section_k0u_wvl_kw4 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_35b_j6q_dhe .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateEdgeInstance。|
|Name|String|是|实例名称。 支持中文汉字、英文大小写、数字、下划线（\_）和连接号（-），不超过20个字符（一个中文汉字算2个字符）。

 |
|Tags|String|否|实例标签。每个标签由key:value组成，多个标签间以逗号隔开。如`k1:v1,k2:v2`。 -   标签key：
    -   不可为空。
    -   在该边缘实例中唯一。
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
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_0sl_ilz_cx9 .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|InstanceId|String|边缘实例ID。|

## 示例 {#section_x5g_0cv_q2i .section}

请求示例

``` {#codeblock_v5f_3lh_ymu}
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateEdgeInstance
&Spec=30
&Tags=k1:v1,k2:v2
&Name=测试实例
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_oep_5s2_3lw}
    {
      "RequestId": "28D159F4-980F-423D-95F0-F705E9DFC016",
      "InstanceId": "F3APY0tPLhmgGtx0****",
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_710_3ep_wdd}
    <?xml version="1.0" encoding="UTF-8" ?>
    <CreateEdgeInstanceResponse>
        <RequestId>28D159F4-980F-423D-95F0-F705E9DFC016</RequestId>
        <InstanceId>F3APY0tPLhmgGtx0****</InstanceId>
        <Code>Success</Code>
        <Success>true</Success>
    </CreateEdgeInstanceResponse>
    ```


