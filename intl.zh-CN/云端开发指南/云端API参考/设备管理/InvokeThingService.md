# InvokeThingService {#reference_snk_mrz_wdb .reference}

调用该接口在一个设备上执行指定的服务。

## 请求参数 {#section_rpk_l1t_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：InvokeThingService。|
|IotId|String|否| 设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，和ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。不能传入空参数。

 |
|ProductKey|String|否| 设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Identifier|String|是|服务的标识符。高级版设备的服务Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。|
|Args|String|是|要启用服务的入参信息，如， Args=\{"param1":1\}。Args详情参见下表[Args](intl.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingService.md#table_gzl_z1t_xdb)。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String| 输入参数的标识符。您在创建该服务时，设置的输入参数的标识符。

 您可以在控制台设备所属的高级版产品的功能定义页面，从该产品的物模型中查看，或单击该服务对应的**编辑**按钮，然后查看您设置的输入参数的信息。

 |
|value|Object|指定参数值。该值须在您设置的输入参数的取值范围内 。|

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
&Args=%7B%22param1%22%3A1%7D
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


