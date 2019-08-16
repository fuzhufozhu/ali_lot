# QueryEdgeInstanceDevice {#reference_1236756 .reference}

查询边缘实例中的设备列表。

## 限制说明 {#section_4bf_81l_6d9 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_18m_apb_dpl .section}

|名称|类型|是否必需|说明|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值QueryEdgeInstanceDevice。|
|InstanceId|String|是|边缘实例ID。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。最小取值是1。如果传入的值小于1，系统按1处理。|
|PageSize|Integer|是|返回结果中，每页显示的记录数量。最大取值30；最小取值是10。如果传入的值小于10，系统按10处理。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_1o8_bzy_ew5 .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|Data|Struct|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Total|Integer|设备数量。|
|PageSize|Integer|返回结果中每页显示的设备数量。|
|CurrentPage|Integer|当前页码。|
|DeviceList|List|设备信息列表。详情请参见下表Device。|

|名称|类型|描述|
|:-|:-|:-|
|IotId|String|设备ID。|
|ProductKey|String|产品的唯一标识符。|
|DeviceName|String|设备名称。|
|DriverId|String|驱动ID。|

## 示例 {#section_741_igf_h43 .section}

请求示例

``` {#codeblock_0sx_2z6_av9}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryEdgeInstanceDevice
&PageSize=15
&InstanceId=tG7sKuOQ7ylb7qS4****
&CurrentPage=1
&公共参数
```

返回示例

-   JSON格式

    ``` {#codeblock_pg8_hev_v0q}
    {
      "RequestId": "AC76932E-E0C9-41EE-843D-B1CA3279B053",
      "Data": {
        "PageSize": 15,
        "CurrentPage": 1,
        "Total": 4,
        "DeviceList": [
          {
            "IotId": "XSpPdtxzE6ebtCkx****000101",
            "DriverId": "44c090ba7b104641a4b9bcf10241****",
            "ProductKey": "a1p5QfE****",
            "DeviceName": "test_tmp_zdy"
          },
          {
            "IotId": "ixX7TRWtewDxZnus****000101",
            "DriverId": "021d154d2a2f4dd7a489773d9e04****",
            "ProductKey": "a1p5QfE****",
            "DeviceName": "test_name19"
          },
          {
            "IotId": "MkzMNGVF3wOk62J6****000101",
            "DriverId": "021d154d2a2f4dd7a489773d9e04****",
            "ProductKey": "a11jTlJ****",
            "DeviceName": "testsss"
          },
          {
            "IotId": "6nFJ9ewglx7aBWPb****000101",
            "DriverId": "021d154d2a2f4dd7a489773d9e04****",
            "ProductKey": "a1PcD3R****",
            "DeviceName": "device01"
          }
        ]
      },
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_ozj_wkr_y4v}
    <?xml version="1.0" encoding="UTF-8"?>
    <QueryEdgeInstanceDeviceResponse>
      <RequestId>AC76932E-E0C9-41EE-843D-B1CA3279B053</RequestId>
      <Data>
        <PageSize>15</PageSize>
        <CurrentPage>1</CurrentPage>
        <Total>4</Total>
        <DeviceList>
          <Device>
            <IotId>XSpPdtxzE6ebtCkx****000101</IotId>
            <DriverId>44c090ba7b104641a4b9bcf10241****</DriverId>
            <ProductKey>a1p5QfE****</ProductKey>
            <DeviceName>test_tmp_zdy</DeviceName>
          </Device>
          <Device>
            <IotId>ixX7TRWtewDxZnus****000101</IotId>
            <DriverId>021d154d2a2f4dd7a489773d9e04****</DriverId>
            <ProductKey>a1p5QfE****</ProductKey>
            <DeviceName>test_name19</DeviceName>
          </Device>
          <Device>
            <IotId>MkzMNGVF3wOk62J6****000101</IotId>
            <DriverId>021d154d2a2f4dd7a489773d9e04****</DriverId>
            <ProductKey>a11jTlJ****</ProductKey>
            <DeviceName>testsss</DeviceName>
          </Device>
          <Device>
            <IotId>6nFJ9ewglx7aBWPb****000101</IotId>
            <DriverId>021d154d2a2f4dd7a489773d9e04****</DriverId>
            <ProductKey>a1PcD3R****</ProductKey>
            <DeviceName>device01</DeviceName>
          </Device>
        </DeviceList>
      </Data>
      <Code>Success</Code>
      <Success>true</Success>
    </QueryEdgeInstanceDeviceResponse>
    ```


