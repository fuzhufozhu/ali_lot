# InvokeThingsService {#reference_ckd_zsj_sfb .reference}

调用该接口批量调用设备服务。

## 使用限制 {#section_f1v_4vj_sfb .section}

-   单账号的每秒请求数最大限制为10 QPS。

    **说明：** 子账号共享主账号配额。

-   目前只支持异步调用该接口。

## 请求参数 {#section_uz4_dzj_sfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：InvokeThingsService。|
|ProductKey|String|是| 要调用服务的设备所隶属的产品Key。

 |
|DeviceNames|List<String\>|是| 要调用服务的设备的名称列表。最多支持100个设备。

 |
|Identifier|String|是|服务的标识符。 设备的服务Identifier，可在控制台中设备所属产品的功能定义中查看。

 |
|Args|String|是| 要启用服务的入参信息，数据格式为JSON String，如， Args=\{"param1":1\}。

 若此参数为空时，需传入 Args=\{\} 。

 Args详情参见下表Args。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String| 输入参数的标识符。您在创建该服务时，设置的输入参数的标识符。

 您可以在控制台设备所属产品的功能定义页面，从该产品的物模型中查看，或单击该服务对应的**编辑**按钮，然后查看您设置的输入参数的信息。

 |
|value|Object|指定参数值。该值须在您设置的输入参数的取值范围内 。|

## 返回参数 {#section_vq2_2bt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_mk3_mbk_sfb .section}

**请求示例**

``` {#codeblock_ggh_jje_384}
https://iot.cn-shanghai.aliyuncs.com/&Action=InvokeThingsService
&Args=%7B%20%20%20%20%20%22walk%22%3A%22a~z%22%2C%20%20%20%20%20%22city%22%3A%22shanghai%22%20%7D
&DeviceName.1=1102andriod02
&DeviceName.2=1102android01
&Identifier=TimeReset
&ProductKey=a1hWjHDWUbF
&公共请求参数
```

**返回示例**

JSON 格式

``` {#codeblock_dgd_ks6_5v5}
{
  "RequestId": "059C3274-6197-4BEC-95E4-49A076330E57",
  "Success": true
}
```

XML 格式

``` {#codeblock_lh4_ie0_sfs}
<?xml version='1.0' encoding='utf-8'?>
<InvokeThingsServiceResponse>
    <RequestId>"059C3274-6197-4BEC-95E4-49A076330E57</RequestId>
    <Success>true</Success>
</InvokeThingsServiceResponse>
```

