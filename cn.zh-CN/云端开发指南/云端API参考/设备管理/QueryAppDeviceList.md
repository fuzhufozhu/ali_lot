# QueryAppDeviceList {#reference_p31_z4j_dfb .reference}

调用该接口查看当前应用所绑定的设备列表。

## 请求参数 {#section_r3l_xpj_dfb .section}

|参数|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryAppDeviceList。|
|AppKey|String|是|应用的AppKey。|
|PageSize|Integer|否|返回结果中每页显示的记录数量，最大值是50。默认值是10。|
|CurrentPage|Integer|否|从返回结果中的第几页开始显示。默认值是 1。|
|ProductKeyList|List<String\>|否|要查询的设备所属的产品ID列表。若不传入此参数，则返回所有绑定该应用的设备列表。|
|TagList|List<TagInfo\>|否|要查询的设备标签。详情请参见下表：TagInfo。|
|公共请求参数|-|是|调用API接口必需的公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|TagName|String|是|指定查询设备的标签名。|
|TagValue|String|是|指定查询设备的标签值。|

## 返回参数 {#section_lbl_w2k_2fb .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页面号。|
|Total|Integer|总记录数。|
|Data|Data|调用成功时，返回的设备信息列表。详情参见下表 DeviceInfo。|

|参数|类型|描述|
|:-|:-|:-|
|ProductName|String|设备隶属的产品名称。|
|ProductKey|String|设备隶属的产品ID。|
|DeviceName|String|设备名称。|
|NodeType|Integer|节点类型，取值： -   0：设备。设备不能挂载子设备，可以直连IoT Hub，也可以作为网关的子设备连接IoT Hub。
-   1：网关。网关可以挂载子设备，具有子设备管理模块，维持子设备的拓扑关系，并且可以将拓扑关系同步到云端。

 |
|Status|String|设备状态。取值：-   ONLINE：设备在线。
-   OFFLINE：设备离线。
-   UNACTIVE：设备未激活。
-   DISABLE：设备已禁用。

 |
|ChildDeviceCount|Long|子设备数量。若设备为网关设备，则会返回此参数。|
|CreateTime|String|设备的创建时间（GMT）。|
|ActiveTime|String|设备的激活时间（GMT）。|
|LastOnlineTime|String|设备的最后上线时间（GMT）。|
|UtcCreateTime|String|设备的创建时间（UTC）。|
|UtcActiveTime|String|设备的激活时间（UTC）。|
|UtcLastOnlineTime|String|设备的最后上线时间（UTC）。|

## 示例 {#section_csc_hgk_2fb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryAppDeviceList
&PageSize=10
&CurrentPage=1
&AppKey=abcabc123
&ProductKeyList.1=al*********
&TagList.1.TagName=room1
&TagList.1.TagValue=light
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "PageCount": 1,
      "Data": {
        "Data": [
          {
            "Status": "ONLINE",
            "DeviceName": "chongla******",
            "UtcCreateTime": "2018-09-15T06:04:51.000Z",
            "ProductKey": "a1TDcaQ59El",
            "UtcActiveTime": "2018-09-15T06:05:52.000Z",
            "LastOnlineTime": "2018-09-25 14:13:46",
            "CreateTime": "2018-09-15 14:04:51",
            "NodeType": 0,
            "ActiveTime": "2018-09-15 14:05:52",
            "ProductName": "cc_test",
            "UtcLastOnlineTime": "2018-09-25T06:13:46.000Z"
          }
        ]
      },
      "Page": 1,
      "PageSize": 10,
      "RequestId": "AE7D1824-A068-4914-8448-E7A5A3E78CAD",
      "Success": true,
      "Total": 1
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="utf-8"?>
    <QueryAppDeviceListResponse>
      <PageCount>1</PageCount>
      <Data>
        <Data>
          <Status>OFFLINE</Status>
          <DeviceName>XPsliHMAF4QNkh******</DeviceName>
          <UtcCreateTime>2018-09-07T07:04:02.000Z</UtcCreateTime>
          <ProductKey>a1b3cVXMgyP</ProductKey>
          <UtcActiveTime>2018-09-07T07:19:36.000Z</UtcActiveTime>
          <LastOnlineTime>2018-09-10 09:47:56</LastOnlineTime>
          <CreateTime>2018-09-07 15:04:02</CreateTime>
          <NodeType>0</NodeType>
          <ActiveTime>2018-09-07 15:19:36</ActiveTime>
          <ProductName>cc_test</ProductName>
          <UtcLastOnlineTime>2018-09-10T01:47:56.000Z</UtcLastOnlineTime>
        </Data>
      </Data>
      <PageSize>10</PageSize>
      <Page>1</Page>
      <RequestId>C67A470A-4AA0-4F9E-A4BE-6B49D2FAFD6D</RequestId>
      <Success>true</Success>
      <Total>1</Total>
    </QueryAppDeviceListResponse>
    ```


