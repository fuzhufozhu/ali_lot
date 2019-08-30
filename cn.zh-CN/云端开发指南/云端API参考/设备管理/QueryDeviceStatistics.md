# QueryDeviceStatistics {#reference_kgv_5rz_wdb .reference}

调用该接口查询设备统计数据。

## 请求参数 {#section_mbr_gwt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceStatistics。|
|ProductKey|String|否|要查询的设备所隶属的产品Key。 -   传入产品Key，将返回该产品下的设备统计数据。
-   不传入该参数，则返回账号下所有设备统计数据。

 |
|IotInstanceId|String|否|公共实例不传此参数；仅独享实例需传入实例ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_cpd_5wt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的设备统计信息。详情参见下表。|

|名称|类型|描述|
|:-|:-|:-|
|deviceCount|Long|设备总数。|
|onlineCount|Long|在线的设备数量。|
|activeCount|Long|已激活的设备数量。|

## 示例 {#section_uvc_jxt_xdb .section}

请求示例

``` {#codeblock_lx6_ot1_s1n}
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceStatistics
&ProductKey=al**********
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_l2b_yqu_rfw}
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "deviceCount": 100,
        "onlineCount": 10,
        "activeCount": 50
      }
    }
    					
    ```

-   XML格式

    ``` {#codeblock_eb4_9r9_13b}
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDeviceStatisticsResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <deviceCount>100</deviceCount>
            <onlineCount>10</onlineCount>
            <activeCount>50</activeCount>
        </Data>
    </QueryDeviceStatisticsResponse>
    ```


