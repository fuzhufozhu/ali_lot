# InvokeThingService {#reference_snk_mrz_wdb .reference}

调用该接口在一个设备上执行指定的服务。

## 请求参数 {#section_rpk_l1t_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：InvokeThingService。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，和ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Identifier|String|是|要查询的事件标识符。高级版设备的事件Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。|
|Args|Map|是|要启用服务的入参信息，详情参见[Args](#table_gzl_z1t_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String|指定参数标识符。|
|value|Object|设置参数值。|

## 返回参数 {#section_vq2_2bt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情,请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情请见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Result|String|同步调用服务返回的调用结果。|
|MessageId|String|云端向设备下发执行服务的消息ID。|

## 示例 {#section_fz1_jbt_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=InvokeThingService
&ProductKey=al*********
&DeviceName=device1
&Identifier=service
&Args=%7B%22service%22%3A1%7D
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true,
      "Data"：{"
    	 "Result": "...."
    	 "MessageId": "abcabc123"
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <InvokeThingServiceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
    	<Data>
    	     <Result>...</Result>
                 <MessageId>abcabc123</MessageId>
    	</Data>
    </InvokeThingServiceResponse>
    ```


