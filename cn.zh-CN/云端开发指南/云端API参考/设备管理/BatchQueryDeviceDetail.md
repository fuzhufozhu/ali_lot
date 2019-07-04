# BatchQueryDeviceDetail {#reference_862489 .reference}

调用该接口批量查询设备详情。

## 限制说明 {#section_zzv_zrq_0f5 .section}

-   只能批量查询当前阿里云账号下的设备详情。如果传入的设备信息中，有设备不属于当前账号，则直接返回失败结果。
-   若传入的设备信息中，包含不存在的设备，则只返回存在的设备详情。
-   该接口单账号的QPS限制为5。
-   单次调用最多能查询100个设备。

## 请求参数 {#section_qi2_0x6_ar9 .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchQueryDeviceDetail。|
|ProductKey|String|是| 要查询的设备所隶属的产品Key。

 |
|DeviceName|List<String\>|是| 指定要查询的设备名称列表。最多可包含100个设备名称。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_i1h_ti9_lm5 .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](cn.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回设备的详细信息。详情请参见下表DeviceInfo。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|设备隶属的产品Key。|
|ProductName|String|设备隶属的产品名称。|
|DeviceName|String|设备名称。|
|Nickname|String|设备的备注名称。|
|DeviceSecret|String|设备密钥。|
|IotId|String|物联网平台为该设备颁发的ID，设备的唯一标识符。|
|UtcCreate|String|​设备的创建时间（UTC）。|
|GmtCreate|String|设备的创建时间（GMT）。|
|UtcActive|String|​​设备的激活时间（UTC）。|
|GmtActive|String|设备的激活时间（GMT）。|
|Status|String| 设备状态。取值：

 ONLINE：设备在线。

 OFFLINE：设备离线。

 UNACTIVE：设备未激活。

 DISABLE：设备已禁用。

 |
|FirmwareVersion|String|设备的固件版本号。|
|NodeType|Integer| 节点类型，取值：

 0：设备。设备不能挂载子设备，可以直接连接到物联网平台，也可以作为网关的子设备连接到物联网平台。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |
|Region|String|设备所在地域（与控制台上物联网平台服务地域对应）。|

## 示例 {#section_c9b_mhz_m6o .section}

**请求示例**

``` {#codeblock_vo4_07c_uaf}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchQueryDeviceDetail
&ProductKey=a1fce6J****
&DeviceName.1=firstDeviceName
&DeviceName.2=secondDeviceName
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_kna_r5a_t16}
    {
      "RequestId": "57b144cf-09fc-4916-a272-a62902d5b787", 
      "Success": true, 
      "Data":[
        {
          "DeviceName": "firstDeviceName",
          "DeviceSecret": "m2gIah1iCkIHXqVJdBVM****",
          "FirmwareVersion": "v1.0.0",
          "GmtCreate": "2019-06-21 20:31:42",
          "GmtActive": "2019-06-21 20:33:00",
          "IotId": "M7aUr3JmdsJ1KQolwI3l00****",
          "Nickname": "TestDevice1",
          "NodeType": 0,
          "ProductKey": "a1fce6J****",
          "ProductName": "testProduct2b2ea8",
          "Region": "cn-shanghai",
          "Status": "ONLINE",
          "UtcCreate": "2019-06-21T12:31:42.000Z",
          "UtcActive": "2019-06-21T12:33:00.000Z",
        },
        {
          "DeviceName": "secondDeviceName",
          "DeviceSecret": "i7nIah1iCkIHXqVJdBVM****",
          "GmtCreate": "2019-06-21 20:31:42",
          "IotId": "ioUyd3JmdsJ1KQolwI3l00****",
          "NodeType": 0,
          "ProductKey": "a1fce6J****",
          "ProductName": "testProduct2b2ea8",
          "Region": "cn-shanghai",
          "Status": "UNACTIVE",
          "UtcCreate": "2019-06-21T12:31:42.000Z"
        }
      ]
    }
    ```

-   XML格式

    ``` {#codeblock_iug_512_r8y}
    <?xml version="1.0" encoding="UTF-8" ?>
    <BatchQueryDeviceDetailResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b787</RequestId>
        <Success>true</Success>
        <Data>
            <DeviceName>firstDeviceName</DeviceName>
            <DeviceSecret>m2gIah1iCkIHXqVJdBVM****</DeviceSecret>
            <FirmwareVersion>v1.0.0</FirmwareVersion>
            <GmtCreate>2019-06-21 20:31:42</GmtCreate>
            <GmtActive>2019-06-21 20:33:00</GmtActive>
            <IotId>M7aUr3JmdsJ1KQolwI3l00****</IotId>
            <Nickname>TestDevice1</Nickname>
            <NodeType>0</NodeType>
            <ProductKey>a1fce6J****</ProductKey>
            <ProductName>testProduct2b2ea8</ProductName>
            <Region>cn-shanghai</Region>
            <Status>ONLINE</Status>
            <UtcCreate>2019-06-21T12:31:42.000Z</UtcCreate>
            <UtcActive>2019-06-21T12:33:00.000Z</UtcActive>
        </Data>
        <Data>
            <DeviceName>secondDeviceName</DeviceName>
            <DeviceSecret>i7nIah1iCkIHXqVJdBVM****</DeviceSecret>
            <GmtCreate>2019-06-21 20:31:42</GmtCreate>
            <IotId>ioUyd3JmdsJ1KQolwI3l00****</IotId>
            <NodeType>0</NodeType>
            <ProductKey>a1fce6J****</ProductKey>
            <ProductName>testProduct2b2ea8</ProductName>
            <Region>cn-shanghai</Region>
            <Status>UNACTIVE</Status>
            <UtcCreate>2019-06-21T12:31:42.000Z</UtcCreate>
        </Data>
    </BatchQueryDeviceDetailResponse>
    ```


