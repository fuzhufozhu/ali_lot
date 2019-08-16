# QueryDeviceByDriver {#reference_878640 .reference}

调用该接口通过驱动查询边缘实例中的设备列表。

## 限制说明 {#section_ckn_hzn_dp8 .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_vm1_6xn_4zw .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceByDriver。|
|InstanceId|String|是|边缘实例ID。|
|DriverId|String|是|驱动ID。|
|PageSize|Integer|是|返回结果中每页显示的记录数量。最大取值30；最小取值10。如果传入的值小于10，系统按10处理。|
|CurrentPage|Integer|是|从返回结果中的第几页开始显示。最小取值 1。如果传入的值小于1，系统按1处理。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_0wy_mul_inj .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|Data|Struct|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Total|Integer|边缘设备数量。|
|PageSize|Integer|返回结果中每页显示的记录数量。|
|CurrentPage|Integer|当前页码。|
|DeviceList|List|边缘设备列表。参数详情请参见下表Device。|

|名称|类型|描述|
|:-|:-|:-|
|IotId|String|设备ID。|
|ProductKey|String|设备所属产品的Key，产品的唯一标识符。|
|DeviceName|String|设备名称。|

## 示例 {#section_5se_ytu_aii .section}

请求示例

``` {#codeblock_8ix_50u_npa}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceByDriver
&DriverId=44c090ba7b104641a4b9bcf10241****
&PageSize=15
&InstanceId=tG7sKuOQ7ylb7qS4****
&CurrentPage=1
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_7jj_bm4_84e}
    {
      "RequestId": "BB13746B-AB68-477E-822A-508B08FC1E0D",
      "Data": {
        "PageSize": 15,
        "CurrentPage": 1,
        "Total": 1,
        "DeviceList": [
          {
            "IotId": "XSpPdtxzE6ebtCkx****000101",
            "ProductKey": "a1p5QfE****",
            "DeviceName": "test_tmp_zdy"
          }
        ]
      },
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_2me_7mx_7dj}
    <?xml version="1.0" encoding="UTF-8"?>
    <QueryDeviceByDriverResponse>
        <RequestId>BB13746B-AB68-477E-822A-508B08FC1E0D</RequestId>
        <Data>
            <PageSize>15</PageSize>
            <CurrentPage>1</CurrentPage>
            <Total>1</Total>
            <DeviceList>
                <Device>
                    <IotId>XSpPdtxzE6ebtCkx****000101</IotId>
                    <ProductKey>a1p5QfE****</ProductKey>
                    <DeviceName>test_tmp_zdy</DeviceName>
                </Device>
            </DeviceList>
        </Data>
        <Code>Success</Code>
        <Success>true</Success>
    </QueryDeviceByDriverResponse>
    ```


