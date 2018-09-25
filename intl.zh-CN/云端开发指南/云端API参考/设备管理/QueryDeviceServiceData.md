# QueryDeviceServiceData {#reference_j2n_lrz_wdb .reference}

调用该接口查询指定设备的服务记录。

## 请求参数 {#section_i1h_sws_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceServiceData。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey&DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品ID。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Identifier|String|是|要查询的服务标识符。高级版设备的服务Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。|
|StartTime|Long|是|要查询的服务记录的开始时间。取值为毫秒值时间戳。|
|EndTime|Long|是|要查询的服务记录的结束时间。取值为毫秒值时间戳。|
|PageSize|Integer|否|返回结果中每页显示的记录数。数量限制：每页最多可显示50条。|
|Asc|Integer|否| 返回结果中服务记录的排序方式，取值：

 0：倒序。

 1：正序。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_uy3_hxs_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|Data|调用成功时，返回的设备服务记录。详情参见[Data](#table_sdc_597r_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|List|List<ServiceInfo\>|事件集合。每个元素代表一个服务。元素的结构参见[ServiceInfo](#table_o4z_bys_xdb)。|
|NextValid|Boolean|表示下一页面是否可用。true表示可用，false表示不可用。|
|NextTime|Long|下一页面中的服务记录的起始时间。|

|名称|类型|描述|
|:-|:-|:-|
|Name|String|服务名称。|
|Time|Long|服务执行的时间。|
|OutputData|String|服务的输出参数，map格式的字符串，结构为 key:value。|
|InputData|String|服务的输入参数，map格式的字符串，结构为 key:value。|
|Identifier|String|服务标识符。|

## 示例 {#section_hn4_3zs_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceServiceData
&IotId=SR8FiTu1R9tlUR2V1bmi0010*****
&ProductKey=al**********
&DeviceName=device1
&Identifier=set
&StartTime=1516538300303L
&EndTime=1516541900303L
&PageSize=10
&Asc=1
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "NextValid": true,
        "NextTime": 1517315865197,
        "List": {
          "ServiceInfo": [
            {
              "Name": "set",
              "Time": 1517315865198,
              "OutputData": "{\"code\":200,\"data\":{},\"id\":\"100686\",\"message\":\"success\",\"version\":\"1.0\"}",
              "InputData": "{\"LightAdjustLevel\":123}",
              "Identifier": "set"
            }
          ]
        }
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDeviceServiceDataResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <NextValid>true</NextValid>
            <NextTime>1517315865197</NextTime>
            <List>
                <ServiceInfo>
                    <Name>set</Name>
                    <Time>1517315865198</Time>
                    <OutputData>{"code":200,"data":{},"id":"100686","message":"success","version":"1.0"}</OutputData>
                    <InputData>{"LightAdjustLevel":123}</InputData>
                    <Identifier>set</Identifier>
                </ServiceInfo>
            </List>
        </Data>
    </QueryDeviceServiceDataResponse>
    ```


