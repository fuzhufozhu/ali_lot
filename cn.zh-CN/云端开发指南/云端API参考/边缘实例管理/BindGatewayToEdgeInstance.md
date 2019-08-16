# BindGatewayToEdgeInstance {#reference_878618 .reference}

调用该接口将网关分配到边缘实例中。

## 限制说明 {#section_ihj_peo_a97 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_u4r_ehs_rsk .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BindGatewayToEdgeInstance。|
|InstanceId|String|是|边缘实例ID。|
|IotId|String|否|网关设备的ID，物联网平台为设备生成的唯一标识符。 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey&DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否|网关所属的产品Key，产品的唯一标识符。 如果传入此参数，需同时传入DeviceName。

 |
|DeviceName|String|否|网关设备名称。 如果传入此参数，需同时传入ProductKey。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_0tl_p53_3ad .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|

## 示例 {#section_55m_r42_2fz .section}

请求示例

``` {#codeblock_o54_ig8_8cc}
https://iot.cn-shanghai.aliyuncs.com/?Action=BindGatewayToEdgeInstance
&InstanceId=F3APY0tPLhmgGtx0****
&ProductKey=a1mAdeG****
&DeviceName=e46ea1a4347c42a0a83b8c956ab1ab
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_r0g_qgt_rke}
    {
      "RequestId": "E3817065-2A17-4814-82FA-66FAB2CC01DF",
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_830_wcd_nye}
    <?xml version="1.0" encoding="UTF-8"?>
    <BindGatewayToEdgeInstanceResponse>
        <RequestId>E3817065-2A17-4814-82FA-66FAB2CC01DF</RequestId>
        <Code>Success</Code>
        <Success>true</Success>
    </BindGatewayToEdgeInstanceResponse>
    ```


