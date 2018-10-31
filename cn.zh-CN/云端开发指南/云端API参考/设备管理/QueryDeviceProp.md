# QueryDeviceProp {#reference_f5j_prz_wdb .reference}

调用该接口查询指定设备的标签列表。

## 请求参数 {#section_uw5_kgt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceProp。|
|ProductKey|String|是|要查询的设备所隶属的产品Key。|
|DeviceName|String|是|要查询的设备的名称。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_v43_5gt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Props|String|调用成功时，返回的设备标签信息列表。|

## 示例 {#section_xrq_1ht_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceProp
&ProductKey=al*********
&DeviceName=device1
&公共请求参数
```

**返回示例**

-   JSON 格式

    ```
    {
    　　"Props":"{"firmwareVersion":"12","color":"red"}",
    　　"RequestId":"F28BDA06-D8DB-4F70-BAED-DDA59AD6EF46",
    　　"Success":true
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
    <QueryDevicePropResponse>
        <RequestId>F28BDA06-D8DB-4F70-BAED-DDA59AD6EF46</RequestId>
        <Success>true</Success>
        <Props>{"firmwareVersion":"12","color":"red"}</Props>
    </QueryDevicePropResponse>
    ```


