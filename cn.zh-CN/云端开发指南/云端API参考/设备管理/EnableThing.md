# EnableThing {#reference_xkt_drz_wdb .reference}

调用该接口解除指定设备的禁用状态，即启用被禁用的设备。

## 限制说明 {#section_3ef_4d8_j5f .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为500。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_u1b_qwm_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：EnableThing。|
|IotId|String|否| 要解除禁用的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要解除禁用的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要解除禁用的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|IotInstanceId|String|否|共享实例用户不传此参数；仅独享实例用户需传入实例ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_u5c_nxm_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_ezy_5xm_xdb .section}

请求示例

``` {#codeblock_t5d_utk_it5}
https://iot.cn-shanghai.aliyuncs.com/?Action=EnableThing
&IotId=SR8FiTu1R9tlUR2V1bmi0010a5****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_j9r_56t_hn8}
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_rpy_piw_dpe}
    <?xml version='1.0' encoding='utf-8'?>
    <EnableThingResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
    </EnableThingResponse>
    ```


