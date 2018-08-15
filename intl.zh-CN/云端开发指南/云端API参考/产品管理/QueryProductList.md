# QueryProductList {#reference_tts_44z_wdb .reference}

调用该接口查看所有产品列表。

## 限制说明 {#section_jgs_wpf_xdb .section}

该接口可用于查看当前账号下所有的IoT产品列表。

## 请求参数 {#section_vyp_zpf_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryProductList。|
|CurrentPage|Integer|是|指定从返回结果中的第几页开始显示。|
|PageSize|Integer|是|指定返回结果中每页显示的记录数量，最大值是200。|
|AliyunCommodityCode|String|否| 指定要查看的产品类型，取值：

 iothub\_senior：物联网平台高级版

 iothub：物联网平台基础版

 **说明：** 如果不传入该参数，则返回所有产品的列表。

 |

## 返回参数 {#section_omg_vqf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|Data|调用成功时，返回的产品列表。详情参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|PageSize|Integer|每页显示的记录数。|
|PageCount|Integer|总页数。|
|CurrentPage|Integer|当前页号。|
|Total|Integer|总记录数。|
|List|List|产品信息列表。请参见下表[ProductInfo](intl.zh-CN/云端开发指南/云端API参考/产品管理/QueryProductList.md#table_zw2_xrf_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|DataFormat|Integer| 数据格式，指设备与云端之间的数据通信协议类型，取值：

 0：透传模式。使用自定义的串口数据格式。该模式下，设备可以上报原始数据（如二进制数据流），阿里云IoT平台会运行您配置在云端的数据解析脚本，将原始数据转换成Alink JSON标准数据格式。

 1：Alink JSON。阿里云IoT平台定义的设备与云端的数据交换协议，采用 JSON 格式。

 **说明：** 此参数为高级版产品特有参数。

 |
|ProductKey|String|产品的Key。新建产品时，IoT为该产品颁发的全局唯一标识。|
|NodeType|Integer| 产品的节点类型，取值：

 0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。

 1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |
|ProductName|String|产品名称。|
|DeviceCount|Integer|产品下的设备数量。|
|GmtCreate|String|产品的创建时间。|
|Description|String|产品的描述信息。|

## 示例 {#section_p5y_rsf_xdb .section}

**请求示例**

```

https://iot.cn-shanghai.aliyuncs.com/?Action=QueryProductList
&CurrentPage=1
&PageSize=10
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
          "Success": true，
          "Data": {
              "PageCount": 4,
              "PageSize": 20,
              "List": {
                  "ProductInfo": [
                      {
                          "DataFormat": 0,
                          "ProductKey": "a1*********",
                          "NodeType": 0,
                          "ProductName": "test123123123",
                          "DeviceCount": 4,
                          "GmtCreate": 1517479077000,
                          "Description": "描述信息"
                      },
                      {
                          "DataFormat": 1,
                          "ProductKey": "a1*********",
                          "NodeType": 0,
                          "ProductName": "测试事件",
                          "DeviceCount": 0,
                          "GmtCreate": 1517466635000
                          "Description": "描述信息"
                      }
                  ]
              },
              "CurrentPage": 1,
              "Total": 79
          }
      }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?>
     <QueryProductListResponse>
          <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
          <Success>true</Success>
          <Data>
              <PageCount>4</PageCount>
              <PageSize>20</PageSize>
              <List>
                  <ProductInfo>
                      <DataFormat>0</DataFormat>
                      <ProductKey>a1*********</ProductKey>
                      <NodeType>0</NodeType>
                      <ProductName>test123123123</ProductName>
                      <DeviceCount>4</DeviceCount>
                      <GmtCreate>1517479077000</GmtCreate>
                      <Description>描述信息</Description>
                  </ProductInfo>
                  <ProductInfo>
                      <DataFormat>1</DataFormat>
                      <ProductKey>a1*********</ProductKey>
                      <NodeType>0</NodeType>
                      <ProductName>测试事件</ProductName>
                      <DeviceCount>0</DeviceCount>
                      <GmtCreate>1517466635000</GmtCreate>
                      <Description>描述信息</Description>
                  </ProductInfo>
              </List>
              <CurrentPage>1</CurrentPage>
              <Total>79</Total>
          </Data>
      </QueryProductListResponse>
    ```


