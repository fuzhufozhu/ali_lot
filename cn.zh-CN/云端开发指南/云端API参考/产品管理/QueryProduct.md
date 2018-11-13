# QueryProduct {#reference_tts_44z_wdb .reference}

调用该接口查询指定产品的详细信息。

## 限制说明 {#section_knr_nlf_xdb .section}

该接口的调用有限流，不得超过50次/秒。

## 请求参数 {#section_e54_rlf_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryProduct。|
|ProductKey|String|是|要查询的产品的ProductKey。ProductKey是物联网平台为新建产品颁发的产品Key，作为其全局唯一标识符。您可以在创建产品的返回结果中查看该信息。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_jbf_mmf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情见表格Data。|

|名称|类型|描述|
|:-|:-|:-|
|GmtCreate|Long|产品创建时间。|
|DataFormat|Integer| 高级版产品的数据类型，指设备与云端之间的数据通信协议类型。取值：

 0：透传模式。使用自定义的串口数据格式。该模式下，设备可以上报原始数据（如二进制数据流）。阿里云IoT平台会运行您配置在云端的数据解析脚本，将原始数据转换成Alink JSON标准数据格式。

 1：Alink JSON。阿里云IoT平台定义的设备与云端的数据交换协议，采用 JSON 格式。

 **说明：** 此参数为高级版产品特有参数。

 |
|Description|String|产品的描述信息。|
|DeviceCount|Integer|该产品下的设备数量。|
|NodeType|Integer| 高级版产品的节点类型。取值：

 0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |
|ProductKey|String|产品Key。新建产品时，IoT平台为该产品颁发的全局唯一标识。|
|ProductName|String|产品名称。|
|ProductSecret|String|产品秘钥。|
|CategoryName|String|高级版产品的设备类型。取值为您在创建高级版产品时，所选择的设备类型。|
|CategoryKey|String|高级版产品的设备类型的英文标识符。|
|AliyunCommodityCode|String|取值：-   iothub：物联网平台基础版 。
-   iothub\_senior：物联网平台高级版。

|
|Id2|Boolean|是否为ID²产品（目前尚未开放）。|

## 示例 {#section_tvz_rnf_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryProduct
&ProductKey=al*********
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
        "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
        "Success": true，
        "Data": {
            "DataFormat": 1,
            "ProductKey": "a1*******",
            "NodeType": 0,
            "ProductName": "iot_test",
    	"ProductSecret":"4alWjhgiTG****"
    	"CategoryName": "",
    	"CategoryKey": "",
            "DeviceCount": 7221,
            "GmtCreate": 1516534408000,
    	"AliyunCommodityCode":"iothub_senior",
            "Description": "Test"
        }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
    <QueryProductResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <DataFormat>1</DataFormat>
            <ProductKey>a1*********</ProductKey>
            <NodeType>0</NodeType>
            <ProductName>iot_test</ProducName>
    	<ProductSecret>4alWjhgiTGU*****</Productecret>
    	<CategoryName></CategryName>
    	<CategoryKey></CategoryKey>
            <DeviceCount>7221</DeviceCount>
            <GmtCreate>1516534408000</GmtCreate>
            <AliyunCommodityCode>iothub_senior</AliyunCommodityCode>
            <Description>Test</Description>
        </Data>
    </QueryProductResponse>
    ```


