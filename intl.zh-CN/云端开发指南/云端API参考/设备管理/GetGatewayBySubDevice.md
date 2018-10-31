# GetGatewayBySubDevice {#reference_vkn_rpj_s2b .reference}

调用该接口，根据挂载的子设备信息，查询对应的网关设备信息。

## 请求参数 {#section_dcg_ttj_s2b .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetGatewayBySubDevice。|
|IotId|String|否| 子设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 子设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 子设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_xh5_r5j_s2b .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的网关设备的详细信息。详情请参见下表[DeviceDetailInfo](#table_mxw_wvj_s2b)。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|网关设备隶属的产品Key。|
|ProductName|String|网关设备隶属的产品名称。|
|DeviceName|String|网关设备名称。|
|DeviceSecret|String|网关设备密钥。|
|IotId|String|IoT平台为该网关设备颁发的ID，作为该设备的唯一标识符。|
|GmtCreate|String|网关设备的创建时间，GMT时间。|
|GmtActive|String|网关设备的激活时间，GMT时间。|
|GmtOnline|String|网关设备最近一次上线的时间，GMT时间。|
|UtcCreate|String|网关设备的创建时间，UTC时间。|
|UtcActive|String|网关设备的激活时间，UTC时间。|
|UtcOnline|String|网关设备最近一次上线的时间，UTC时间。|
|Status|String| 网关设备状态。取值：

 ONLINE：设备在线。

 OFFLINE：设备离线。

 UNACTIVE：设备未激活。

 DISABLE：设备已禁用。

 |
|FirmwareVersion|String|网关设备的固件版本号。|
|IpAddress|String|网关设备的IP地址。|
|NodeType|Integer|节点类型，取值为1，表示该设备为网关设备。|
|Region|String|网关设备所在地域（与控制台上的地域对应）。|

## 示例 {#section_qkh_hxj_s2b .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetGatewayBySubDevice
&ProductKey=al*********S
&DeviceName=XTzosqEOgxFXKPRgd8zl
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
    "RequestId": "F227A41E-8A0F-4829-A1B1-727619DB58A3",
        "Data": 
        {
            "Status": "UNACTIVE",
            "ProductName": "TEST",
            "DeviceSecret": "nICOJkFJnG***********TWnvXHydEjX",
            "UtcOnline": "",
            "IotId": "9a1MTdk9brqQ2bhdG4OY001094fd00",
            "GmtCreate": "2018-08-07 15:54:15",
            "UtcCreate": "2018-08-07T07:54:15.000Z",
            "UtcActive": "",
            "GmtActive": "",
            "NodeType": 1,
            "Region": "cn-shanghai",
            "GmtOnline": "",
            "ProductKey": "a15tzTmkWZ2",
            "DeviceName": "d896e0ff000105bb"
            "IpAddress": "10.0.0.1"
            "FirmwareVersion": "1.2.3"
        },
    "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <GetGatewayBySubDeviceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <Status>UNACTIVE</Status>
            <ProductName>TEST</ProductName>
            <DeviceSecret>nICOJkFJnG***********TWnvXHydEjX</DeviceSecret>
            <UtcOnline></UtcOnline>
            <IotId>SR8FiTu1R9tlUR2V1bmi0010******</IotId>
            <GmtCreate>2018-08-07 15:54:15</GmtCreate>
            <UtcCreate>2018-08-07T07:54:15.000Z</GmtOnline>
            <UtcActive></UtcActive>
            <GmtActive></GmtActive>
            <NodeType>1</NodeType>
            <Region>cn-shanghai</Region>
            <GmtOnline></GmtOnline>
            <ProductKey>a15tzTmkWZ2</ProductKey>
            <DeviceName>TEST</DeviceName>
            <IpAddress>10.0.0.1</IpAddress>
            <FirmwareVersion>1.2.3</FirmwareVersion>
        </Data>
    </GetGatewayBySubDeviceResponse>
    ```


