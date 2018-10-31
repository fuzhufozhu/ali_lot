# QueryDeviceDetail {#reference_rrg_lpz_wdb .reference}

调用该接口查询指定设备的详细信息。

## 请求参数 {#section_wj4_4wl_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceDetail。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，和ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 指定要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_v2t_4xl_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回设备的详细信息。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|设备隶属的产品Key。|
|ProductName|String|设备隶属的产品名称。|
|DeviceName|String|设备名称。|
|DeviceSecret|String|设备密钥。|
|IotId|String|IoT平台为该设备颁发的ID，作为该设备的唯一标识符。|
|UtcCreate|String|​设备的创建时间（UTC）。|
|GmtCreate|String|设备的创建时间（GMT）。|
|UtcActive|String|​​设备的激活时间（UTC）。|
|GmtActive|String|设备的激活时间（GMT）。|
|UtcOnline|String|​​设备最近一次上线的时间（UTC）。|
|GmtOnline|String|设备最近一次上线的时间（GMT）。|
|Status|String| 设备状态。取值：

 ONLINE：设备在线。

 OFFLINE：设备离线。

 UNACTIVE：设备未激活。

 DISABLE：设备已禁用。

 |
|FirmwareVersion|String|设备的固件版本号。|
|IpAddress|String|设备的IP地址。|
|NodeType|Integer| 节点类型，取值：

 0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |
|Region|String|设备所在地区（与控制台上的物联网地平台服务地域对应）。|

## 示例 {#section_gm1_hyl_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceDetail
&ProductKey=a1rYuV*****
&DeviceName=SR8FiTu1R9tlUR2V1bmi
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "DeviceName": "SR8FiTu1R9tlUR2V1bmi",
        "GmtActive": "2018-08-06 10:48:41",
        "UtcActive": "2018-08-06T02:48:41.000Z",
        "ProductKey": "a1rYuV*****",
        "DeviceSecret": "CPwUjMUgzdvaZv56TMy6773V3v3****",
        "GmtCreate": "2018-08-06 10:47:50",
        "UtcCreate": "2018-08-06T02:47:50.000Z",
        "IotId": "SR8FiTu1R9tlUR2V1bmi0010*****",
        "Status": "ONLINE",
        "Region": "cn-shanghai",
        "NodeType": 0,
        "GmtOnline": "2018-08-06 13:43:12",
        "UtcOnline": "2018-08-06T05:43:12.000Z",
        "ProductName": "test",
        "IpAddress":"10.0.0.1",
        "FirmwareVersion":"V1.0"
      },
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDeviceDetailResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <DeviceName>SR8FiTu1R9tlUR2V1bmi</DeviceName>
            <GmtActive>2018-08-06 10:48:41</GmtActve>
    	<UtcActive>2018-08-06T02:48:41.000Z</UtcActive>
            <ProductKey>a1rYuVF****</ProductKey>
            <DeviceSecret>CPwUjMUgzdvaZv56TMy6773V3v3W****</DeviceSecret>
            <GmtCreate>2018-08-06 10:47:50</GmCreate>
    	<UtcCreate>2018-08-06T02:47:50.000Z</UtcCreate>
            <IotId>SR8FiTu1R9tlUR2V1bmi0010a5****</IotId>
            <Status>ONLINE</Status>
            <Region>cn-shanghai</Region>
            <NodeType>0</NodeType>
            <GmtOnline>2018-08-06 13:43:2</GmtOnline>
    	<UtcOnline>2018-08-06T05:43:12.000Z</UtcOnline>
            <ProductName>test</ProductName>
            <IpAddress>10.0.0.1</IpAddress>        
            <FirmwareVersion>V1.0</FirmwareVersion>
        </Data>
    </QueryDeviceDetailResponse>
    ```


