# DeleteProduct {#reference_tkz_vxn_1fb .reference}

调用该接口删除指定产品。

## 接口说明 {#section_elb_xr4_1fb .section}

如果您不再需要某个产品，您可以将其删除。产品删除后，产品Key（ProductKey）将失效，与产品关联的其他信息也一并删除，您将无法执行与该产品关联的任何操作。

## 请求参数 {#section_el4_ls4_1fb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteProduct。|
|ProductKey|String|是|要删除的产品Key。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_qsw_xs4_1fb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_szq_mt4_1fb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteProduct
&ProductKey=a1QDKAU****
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?>
     <DeleteDeviceResponse>
          <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
          <Success>true</Success>
      </DeleteDeviceResponse>
    ```


