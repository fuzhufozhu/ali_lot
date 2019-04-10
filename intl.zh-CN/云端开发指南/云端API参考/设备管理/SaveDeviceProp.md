# SaveDeviceProp {#reference_xd3_qrz_wdb .reference}

调用该接口为指定设备设置标签。

## 请求参数 {#section_gvp_qht_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：SaveDeviceProp。|
|ProductKey|String|否| 要创建标签的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要创建标签的设备名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|IotId|String|否| 设备ID，物联网平台为设备颁发的唯一标识。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey & DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|Props|String|是| 要设置的设备标签，每个标签的具体结构参见[Prop](#table_rxn_33t_xdb)。

 **说明：** 

-   设备标签是JSON格式，Props是对应的String格式。Prop由标签key和value组成，key和value均不能为空。如，Props=\{"color":"red"\}。
-   Props的总大小不超过5K。
-   单个设备的设备标签总数不超过100个。
-   单次修改或新增的设备标签数不超过100个。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|key|String|是| 设置标签键值。标签键值可包含英文字母，数字和点号（.），长度在2-30字符之间。

 如果设置的键值已存在，则将覆盖之前的标签值。

 如果设置的键值不存在，则新增标签。

 |
|value|Object|是|标签值。可包含中文、英文字母、数字、下划线（\_）、连字符（-）和点号（.）。长度不可超过128字符。一个中文汉字算2字符。|

## 返回参数 {#section_ydb_gjt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_srv_mjt_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=SaveDeviceProp
&ProductKey=al**********
&DeviceName=device1
&Props=%7B%22color%22%3A%22red%22%7D
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
    <SaveDevicePropResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
    </SaveDevicePropResponse>
    ```


