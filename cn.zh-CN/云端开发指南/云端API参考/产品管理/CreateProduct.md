# CreateProduct {#reference_mjn_n5l_vdb .reference}

调用该接口新建产品。

## 请求参数 {#section_ekq_55l_vdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateProduct。|
|ProductName|String|是|为新建产品命名。产品名应满足以下限制：由4-30位中文、英文字母、数字和下划线（\_）组成（一个中文字符占两位）。 **说明：** 产品名在当前账号下应保持唯一。

 |
|NodeType|Integer|是| 产品的节点类型，取值：

 0：设备。设备不能挂载子设备。可以直连物联网平台，也可以作为网关的子设备连接物联网平台。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，和将拓扑关系同步到物联网平台。

 |
|AliyunCommodityCode|String|否|产品版本类型。 -   iothub\_senior：高级版。
-   iothub：基础版。

 若不传入此参数默，则默认为基础版。

 |
|DataFormat|Integer|是|产品版本类型选择为iothub\_senior的产品数据格式。 此参数为高级版产品的特有参数，并且是创建高级版产品的必需参数。

 可选值：

 -   0：透传/自定义格式（CUSTOM\_FORMAT）。
-   1：Alink协议（ALINK\_FORMAT）。

 |
|Description|String|否|为新建产品添加描述信息。描述信息应在100字符以内。|
|Id2|Boolean|否|是否使用ID²认证。 可选值：

 -   true：开通ID²认证。
-   false：不开通ID²认证。

 不传入此参数，则默认为不开通。

 |
|ProtocolType|String|否|设备接入网关的协议类型。 此参数为创建高级版产品，且产品节点类型为要接入网关的设备时的特有参数。

 可选值：

 -   modbus：Modbus协议。
-   opc-ua：OPC UA协议。
-   customize：自定义协议。
-   ble：BLE协议。
-   zigbee:ZigBee协议。

 |
|NetType|String|否|联网方式。 此参数为创建高级版产品，产品节点类型为网关或不接入网关的设备时的特有参数。

 可选值：

 -   WIFI: WiFi
-   CELLULAR：蜂窝网
-   ETHERNET：以太网
-   LORA：LoRaWAN
-   OTHER：其他

 若不传入此参数，则默认为WiFi。

 |
|JoinPermissionId|String|否|LoRaWAN入网凭证ID。连网方式NetType选择为LORA时，该参数必需。 请调用[QueryLoRaJoinPermissions](cn.zh-CN/云端开发指南/云端API参考/设备管理/QueryLoRaJoinPermissions.md#)查询您账号下的LoRaWAN入网凭证的JoinPermissionId。

 如果您还没有LoRaWAN入网凭证，请访问[物联网络管理平台](https://linkwan.console.aliyun.com/join-permission-authorization)创建。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_jmn_bvl_vdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见本文错误码章节。|
|ProductKey|String|产品的Key。|
|Data|Data|调用成功时返回的新建产品信息。详情参见下表ProductInfo。|

|名称|类型|描述|
|:-|:-|:-|
|ProductName|String|产品的名称。|
|ProductKey|String|物联网平台为新建产品颁发的产品Key，作为该产品的全局唯一标识。 **说明：** 请妥善保管新建产品的ProductKey。在其他操作中会用到该信息。

 |
|Description|String|产品描述信息。|
|DataFormat|Integer|产品类型数据格式。 -   0：透传/自定义格式（CUSTOM\_FORMAT）。
-   1：Alink协议（ALINK\_FORMAT）。

 此参数为高级版产品的特有参数。

 |
|AliyunCommodityCode|String|产品类型。 -   iothub\_senior：高级版。
-   iothub：基础版。

 |
|ProtocolType|String|设备接入网关协议类型。 此参数为高级版产品的特有参数。

 |
|NodeType|Integer| 产品的节点类型，取值：

 0：设备。设备不能挂载子设备。可以直连物联网平台，也可以作为网关的子设备连接物联网平台。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，和将拓扑关系同步到物联网平台。

 此参数为高级版产品的特有参数。

 |

## 示例 {#section_c1k_qvl_vdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateProduct
&AliyunCommodityCode=iothub_senior
&DataFormat=1
&Description=Product test
&NodeType=0
&ProductName=Test
&ProtocolType=modbus
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_wyj_uyb_34z}
    {
    
        "Data": {
    
            "Description": "Product test", 
    
            "DataFormat": 1, 
    
            "ProtocolType": "modbus", 
    
            "ProductKey": "a1FlqIQ****", 
    
            "NodeType": 0, 
    
            "ProductName": "Test", 
    
            "AliyunCommodityCode": "iothub_senior"
    
        }, 
    
        "ProductKey": "a1FlqIQ****", 
    
        "RequestId": "E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565", 
    
        "Success": true
    
    }
    ```

-   XML格式

    ``` {#codeblock_mw3_4q5_ugf}
    <?xml version="1.0" encoding="UTF-8" ?>
    <CreateProductResponse>
    	<Data>
    		<Description>Product test</Description>
    		<DataFormat>1</DataFormat>
    		<ProtocolType>modbus</ProtocolType>
    		<ProductKey>a1FlqIQ****</ProductKey>
    		<NodeType>0</NodeType>
    		<ProductName>Test</ProductName>
    		<AliyunCommodityCode>iothub_senior</AliyunCommodityCode>
    	</Data>
    	<ProductKey>a1FlqIQ****</ProductKey>
    	<RequestId>E55E50B7-40EE-4B6B-8BBE-D3ED55CCF565</RequestId>
    	<Success>true</Success>
    </CreateProductResponse>
    ```


