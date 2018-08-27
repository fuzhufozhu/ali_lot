# CreateProduct {#reference_mjn_n5l_vdb .reference}

调用该接口新建产品。

## 限制说明 {#section_gch_t5l_vdb .section}

该接口只适用于新建基础版产品 。目前，高级版产品需通过物联网平台控制台创建。

## 请求参数 {#section_ekq_55l_vdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateProduct。|
|Name|String|是|为新建产品命名。产品名应满足以下限制：由4-30位中文、英文字母、数字和下划线组成（一个中文字符占两位）。**说明：** 产品名在当前账号下应保持唯一。

|
|NodeType|Integer|否| 产品的节点类型，取值：

 0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 如果不传入该参数，则使用默认值0。

 |
|CatId|Long|否|类目信息。**说明：** 该字段是旧版本中遗留的字段，没有实际意义，您可以将其指定为任意的长整型数，例如10000。

|
|Desc|String|否|为新建产品添加描述信息。描述信息应在100字符以内。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_jmn_bvl_vdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时返回的出错信息。|
|ProductInfo|ProductInfo|调用成功时返回的新建产品信息。详情参见[表 1](#table_z3k_lz2_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|ProductName|String|新建产品的名称。|
|ProductKey|String|IoT为新建产品颁发的产品Key，作为该产品的全局唯一标识。**说明：** 请妥善保管新建产品的ProductKey。在其他操作中会用到该信息。

|
|ProductDesc|String|产品描述信息。|
|CreateUserId|Long|创建者ID。|
|CatId|Long|类目信息。**说明：** 该字段是旧版本中遗留的字段，没有实际意义。

|
|FromSource|String|来源渠道，取值：iothub。|
|NodeType|Integer| 产品的节点类型，取值：

 0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |

## 示例 {#section_c1k_qvl_vdb .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=CreateProduct
&Name=TestProduct
&CatId=10000
&Desc=Create Product test
&公共请求参数


```

**返回示例**

-   JSON格式

    ```
    {
          RequestId:8AE93DAB-958F-49BD-BE45-41353C6DEBCE,
          Success:true, 
          ProductInfo:{
              ProductKey:al*********, 
              CatId:10000, 
    	  NodeType:0,
              ProductName:TestProduct,
    	  ProductDesc:Create Product test
          }
      }}
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?> 
      <CreateProductResponse>
          <RequestId>8AE93DAB-958F-49BD-BE45-41353C6DEBCE</RequestId>
          <ProductInfo>
              <ProductKey>al*********</ProductKey>
              <ProductDesc>Create Product test</ProductDesc>
              <CatId>10000</CatId>
              <ProductName>TestProductx</ProductName>
    		  <NodeType>0</NodeType>
          </ProductInfo>
          <Success>true</Success>
      </CreateProductResponse>
    
    ```


