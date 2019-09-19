# InvokeThingService {#reference_snk_mrz_wdb .reference}

调用该接口在一个设备上调用指定服务。

## 限制说明 {#section_zbx_wrm_2gb .section}

如果同步调用服务，最大超时时间为5秒。若5秒内服务器未收到回复，则返回超时错误；如果异步调用服务，则无最大超时时间限制。

## 请求参数 {#section_rpk_l1t_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：InvokeThingService。|
|IotId|String|否| 要调用服务的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，和ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。不能传入空参数。

 |
|ProductKey|String|否| 要调用服务的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要调用服务的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Identifier|String|是|服务的标识符。设备的服务Identifier，可在控制台中设备所属的产品的功能定义中查看。有关功能定义说明，请参见[单个添加物模型](../../../../intl.zh-CN/用户指南/产品与设备/物模型/单个添加物模型.md#)。|
|Args|String|是| 要启用服务的入参信息，数据格式为JSON String，如， Args=\{"param1":1\}。

 若此参数为空时，需传入 Args=\{\} 。

 Args详情参见下表Args。

 |
|IotInstanceId|String|否|公共实例不传此参数；仅独享实例需传入实例ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String| 输入参数的标识符。您在创建该服务时，设置的输入参数的标识符。

 您可以在控制台设备所属的产品的功能定义页面，从该产品的物模型中查看，或单击该服务对应的**编辑**按钮，然后查看您设置的输入参数的信息。

 |
|value|Object|指定参数值。该值须在您设置的输入参数的取值范围内 。|

## 返回参数 {#section_vq2_2bt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情请见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|Result|String| 同步调用服务，返回的调用结果。

 异步调用服务，不返回此参数。

 |
|MessageId|String|云端向设备下发服务调用的消息ID。|

## 示例 {#section_fz1_jbt_xdb .section}

请求示例

``` {#codeblock_3fj_ppi_gyp}
https://iot.cn-shanghai.aliyuncs.com/?Action=InvokeThingService
&ProductKey=al*********
&DeviceName=device1
&Identifier=service
&Args=%7B%22param1%22%3A1%7D
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_aa8_8u4_oua .language-json}
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

    ``` {#codeblock_5oi_7j6_yh8 .lanuage-xml}
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


