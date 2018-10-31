# RemoveThingTopo {#reference_ocy_trz_wdb .reference}

调用该接口移除网关与子设备的拓扑关系。

## 请求参数 {#section_aym_wtt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：RemoveThingTopo。|
|IotId|String|否| 要删除的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要删除的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要删除的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_dxh_m5t_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Boolean|删除拓扑关系的结果。true表示删除成功，false表示删除失败。|

## 示例 {#section_wjq_t5t_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=RemoveThingTopo
&IotId=CqXL5h5ysRTA4NxjABjj0010fa****
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
        "Data": true,
        "RequestId": "098BEEF4-58F4-4181-A891-5D37DB6C4C3B",
        "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
    <RemoveThingTopoResponse>
        <RequestId>098BEEF4-58F4-4181-A891-5D37DB6C4C</RequestId>
        <Success>true</Success>
        <Data>true</Data>
    </RemoveThingTopoResponse>
    ```


