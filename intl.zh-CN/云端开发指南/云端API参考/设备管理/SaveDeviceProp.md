# SaveDeviceProp {#reference_xd3_qrz_wdb .reference}

调用该接口为指定设备设置标签。

## 请求参数 {#section_gvp_qht_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：SaveDeviceProp。|
|ProductKey|String|是|要操作的设备所隶属的产品ID。|
|DeviceName|String|是|要操作的设备的名称。|
|Props|String|是| 要设置的设备标签，每个标签的具体结构参见[Prop](#table_rxn_33t_xdb)。

 **说明：** 

-   设备标签是JSON格式，Props是对应的string格式。Prop由标签key和value组成，key和value均不能为空。如，Props=\{"color":"red"\}（取值需经URL编码处理，见请求示例）。
-   Props的总大小不超过5K。
-   单个设备的设备标签总数不超过100个。
-   单次修改或新增的设备标签数不超过100个。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Key|String|是|设置标签键值。标签键值只能是英文，且长度在2-32字符之间。如果设置的键值已存在，则将覆盖之前的标签值；如果不存在，则新增标签。|
|Value|Object|是|标签值。|

## 返回参数 {#section_ydb_gjt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

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


