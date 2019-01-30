# QueryDeviceListByDeviceGroup {#reference_psj_jrw_1gb .reference}

调用该接口查询分组中的设备列表。

## 请求参数 {#section_zrz_nrw_1gb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDeviceListByDeviceGroup。|
|GroupId|String|是|分组ID，分组的全局唯一标识符。|
|CurrentPage|Integer|否|指定显示查询结果中的第几页。默认值为1。|
|PageSize|Integer|否|指定返回结果中，每页显示的设备数量。默认值为10。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_sxq_bsw_1gb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Page|Integer|当前页码。|
|PageSize|Integer|每页所显示的设备数量。|
|PageCount|Integer|总页数。|
|Total|Integer|设备总数。|
|Data|List|调用成功时，返回的设备列表信息数据。详情请参见下表SimpleDeviceInfo。|

|名称|类型|描述|
|:-|:-|:-|
|ProductName|String|设备所属的产品名称。|
|ProductKey|String|设备所属的产品Key。|
|DeviceName|String|设备名称。|
|IotId|String|物联网平台为该设备颁发的ID，作为该设备的唯一标识符。|

## 示例 {#section_cyp_wvw_1gb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceListByDeviceGroup
&GroupId=7DIgqIl1IjnhBsrA
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "PageCount": 1,
      "Data": {
        "SimpleDeviceInfo": [
          {
            "DeviceName": "ios_1207_08",
            "ProductKey": "a1hWjHDWUbF",
            "ProductName": "WIFIdevice",
            "IotId": "TfmUAeJjQQhCPH84UVNn0010c66100"
          },
          {
            "DeviceName": "ios_1207_07",
            "ProductKey": "a1hWjHDWUbF",
            "ProductName": "WIFIgateway",
            "IotId": "wVPeAksaboXBlRgvZNHQ0010310800"
          },
          {
            "DeviceName": "E1IPK25iL4CTOwnuI2yt",
            "ProductKey": "a1mV8bKjeP6",
            "ProductName": "yanlv",
            "IotId": "E1IPK25iL4CTOwnuI2yt0010598d00"
          }
        ]
      },
      "PageSize": 10,
      "Page": 1,
      "RequestId": "B1A921D9-1061-4D45-9F12-EA6B0FDEDE30",
      "Success": true,
      "Total": 3
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <QueryDeviceListByDeviceGroup>
    	<PageCount>1</PageCount>
    	<Data>
    		<SimpleDeviceInfo>
    			<DeviceName>ios_1207_08</DeviceName>
    			<ProductKey>a1hWjHDWUbF</ProductKey>
    			<ProductName>WIFIdevice</ProductName>
    			<IotId>TfmUAeJjQQhCPH84UVNn0010c66100</IotId>
    		</SimpleDeviceInfo>
    		<SimpleDeviceInfo>
    			<DeviceName>ios_1207_07</DeviceName>
    			<ProductKey>a1hWjHDWUbF</ProductKey>
    			<ProductName>WIFIgateway</ProductName>
    			<IotId>wVPeAksaboXBlRgvZNHQ0010310800</IotId>
    		</SimpleDeviceInfo>
    		<SimpleDeviceInfo>
    			<DeviceName>E1IPK25iL4CTOwnuI2yt</DeviceName>
    			<ProductKey>a1mV8bKjeP6</ProductKey>
    			<ProductName>yanlv</ProductName>
    			<IotId>E1IPK25iL4CTOwnuI2yt0010598d00</IotId>
    		</SimpleDeviceInfo>
    	</Data>
    	<PageSize>10</PageSize>
    	<Page>1</Page>
    	<RequestId>B1A921D9-1061-4D45-9F12-EA6B0FDEDE30</RequestId>
    	<Success>true</Success>
    	<Total>3</Total>
    </QueryDeviceListByDeviceGroup>
    ```


