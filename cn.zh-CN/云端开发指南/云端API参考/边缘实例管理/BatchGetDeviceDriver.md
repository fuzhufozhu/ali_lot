# BatchGetDeviceDriver {#reference_878636 .reference}

调用该接口批量获取边缘实例设备的驱动。

## 限制说明 {#section_b8o_8b3_cjl .section}

-   单阿里云账号调用该接口的每秒请求数（QPS）最大限制为10。

    **说明：** 子账号共享主账号配额。

-   单客户端出口IP的最大QPS限制为100，即来自单个客户端出口IP，调用阿里云接口的每秒请求总数不能超过100。

## 请求参数 {#section_nil_olx_occ .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchGetDeviceDriver。|
|InstanceId|String|是|边缘实例ID。|
|IotIds|List\(String\)|是|要查询的设备ID列表。 可调用[QueryEdgeInstanceDevice](cn.zh-CN/云端开发指南/云端API参考/边缘实例管理/QueryEdgeInstanceDevice.md#)查询实例中的设备列表。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_r0t_zb2_5ra .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|接口返回码。|
|DeviceDriverList|List|调用成功时，返回的驱动信息。详情请参见下表DeviceDriver。|

|名称|类型|描述|
|:-|:-|:-|
|DriverId|String|驱动ID。|
|IotId|String|设备ID。|

## 示例 {#section_mg8_x2k_i7h .section}

请求示例

``` {#codeblock_shq_zxl_bi2}
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchGetDeviceDriver
&IotIds.1=8n5sFqlfnchlBjct****000101
&InstanceId=F3APY0tPLhmgGtx0****
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_dar_bun_jgv}
    {
      "RequestId": "F8429878-8028-43B8-A541-F5BC956641C2",
      "DeviceDriverList": [
        {
          "IotId": "8n5sFqlfnchlBjct****000101",
          "DriverId": "021d154d2a2f4dd7a489773d9e04****"
        }
      ],
      "Code": "Success",
      "Success": true
    }
    ```

-   XML格式

    ``` {#codeblock_clz_6zx_3cg}
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchGetDeviceDriverResponse>
        <RequestId>F8429878-8028-43B8-A541-F5BC956641C2</RequestId>
      <DeviceDriverList>
        <DeviceDriver>
          <IotId>8n5sFqlfnchlBjct****000101</IotId>
          <DriverId>021d154d2a2f4dd7a489773d9e04****</DriverId>
        </DeviceDriver>
      </DeviceDriverList>
      <Code>Success</Code>
      <Success>true</Success>
    </BatchGetDeviceDriverResponse>
    ```


