# DeleteDeviceProp {#reference_sv2_rrz_wdb .reference}

调用该接口删除设备下的指定标签。

## 请求参数 {#section_hzp_clt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteDeviceProp。|
|IotId|String|否| 要删除标签的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey & DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否|要删除标签的设备所隶属的产品Key。**说明：** 如果传入该参数，需同时传入DeviceName。

|
|DeviceName|String|否|要删除标签的设备的名称。**说明：** 如果传入该参数，需同时传入ProductKey。

|
|PropKey|String|是| 要删除的设备标签键值（Key）。

 **说明：** 物联网平台在目标设备的标签中检索您提供的Key值，并删除与之对应的标签。如果目标设备的标签中没有与您提供的Key值对应的记录，则不执行任何操作。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_f1k_nlt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_ypv_tlt_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteDeviceProp
&ProductKey=al*********
&DeviceName=device1
&PropKey=temperature
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
        "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
        "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <DeleteDevicePropResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
    </DeleteDevicePropResponse>
    ```


