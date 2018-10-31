# QueryDevicePropertyStatus {#reference_rgh_nrz_wdb .reference}

调用该接口查询指定设备的属性快照。

## 请求参数 {#section_rdp_hct_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDevicePropertyStatus。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey & DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|公共请求参数| |是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_ibj_pct_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情参见下表。|

|名称|类型|描述|
|:-|:-|:-|
|List|List|返回的属性集合信息。详情参见下表PropertyStatusInfo。|

|名称|类型|描述|
|:-|:-|:-|
|Identifier|String|属性标识符。|
|Name|String|属性名称。|
|DataType|String| 属性格式类型，取值：

 int：整型。

 float：单精度浮点型。

 double：双精度浮点型。

 enum：枚举型。

 bool：布尔型。

 text：字符型。

 date：时间型（String类型的UTC时间戳，单位是毫秒）。

 array：数组型。

 struct：结构体类型。

 |
|Time|String|属性修改的时间，单位是毫秒。|
|Value|String|属性值。|
|Unit|String|属性单位。|

## 示例 {#section_gqj_ndt_xdb .section}

**请求示例** 

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDevicePropertyStatus
&IotId=SR8FiTu1R9tlUR2V1bmi0010*****
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
        "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
        "Success": true，
        "Data": {
            "List": {
                "PropertyStatusInfo": [
                    {
                        "Name": "doublePropertyName",
                        "Value": "50.0",
                        "Time": "1517553572362",
                        "DataType": "double",
                        "Identifier": "doubleProperty",
                        "Unit": "C"
                    }
                ]
            }
        }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDevicePropertyStatusResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <List>
                <PropertyStatusInfo>
                    <Time>1517553572362</Time>
                    <Name>doublePropertyName</Name>
                    <DataType>double</DataType>
                    <Identifier>doubleProperty</Identifier>
                    <Value>50.0</Value>
                    <Unit>C</Unit>
                </PropertyStatusInfo>
            </List>
        </Data>
    </QueryDevicePropertyStatusResponse>
    ```


