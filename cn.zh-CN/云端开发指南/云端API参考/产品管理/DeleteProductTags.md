# DeleteProductTags {#reference_yrb_ftk_3gb .reference}

调用该接口删除产品标签。

## 限制说明 {#section_slm_p5k_3gb .section}

单次调用该接口最多可删除10个标签。

## 请求参数 {#section_crl_ptk_3gb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值DeleteProductTags。|
|ProductKey|String|是|产品Key，物联网平台为产品颁发的唯一标识。|
|ProductTagKeys|List<String\>|是|产品标签键列表。|
|IotInstanceId|String|否|公共实例不传此参数；仅独享实例需传入实例ID。|
|公共参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_hwv_5kk_3gb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_htz_35k_3gb .section}

请求示例

``` {#codeblock_5vk_5ps_28t}
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteProductTags
&ProductKey=a1h7knJdld1
&ProductTagKey.1=first
&ProductTagKey.2=second
&公共请求参数
```

返回示例

JSON格式

``` {#codeblock_oda_tt9_7dw}
{
  "RequestId": "E7E8456E-EDD7-41D3-83B1-62FF4F5ED6BD",
  "Success": true
}
```

XML格式

``` {#codeblock_w5l_h9c_i80}
<DeleteProductTagsResponse>
    <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
    <Success>true</Success>
</DeleteProductTagsResponse>
```

