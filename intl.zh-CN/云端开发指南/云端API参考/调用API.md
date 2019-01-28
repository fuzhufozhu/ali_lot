# 调用API {#reference_vy5_swc_xdb .reference}

## 请求结构 {#section_qjd_4t5_ydb .section}

您可以通过发送 HTTP /HTTPS 请求调用物联网平台 API。

请求结构如下：

```
http://Endpoint/?Action=xx&Parameters
```

其中：

-   Endpoint：调用的云服务的接入地址。物联网平台的接入地址分别是：
    -   华东2（上海）：`iot.cn-shanghai.aliyuncs.com`
    -   新加坡：`iot.ap-southeast-1.aliyuncs.com`
    -   美国（硅谷）：`iot.us-west-1.aliyuncs.com`
    -   日本（东京）：`iot.ap-northeast-1.aliyuncs.com`
    -   德国（法兰克福）：`iot.eu-central-1.aliyuncs.com`
-   Action：要执行的操作。如使用Pub接口向指定Topic发布消息。
-   Parameters：请求参数。每个参数之间用（&）分隔。

    请求参数由公共请求参数和API自定义参数组成。公共参数中包含API版本号、身份验证等信息。


下面以调用Pub接口向指定Topic发布消息为例：

**说明：** 本文档示例均使用华东2（上海）地域的接入地址。为了便于阅读，本文档中的示例均做了格式化处理。

```
https://iot.cn-shanghai.aliyuncs.com/?Action=Pub
&Format=XML
&Version=2017-04-20
&Signature=Pc5WB8gokVn0xfeu%2FZV%2BiNM1dgI%3D
&SignatureMethod=HMAC-SHA1
&SignatureNonce=15215528852396
&SignatureVersion=1.0
&AccessKeyId=...
&Timestamp=2017-07-19T12:00:00Z
&RegionId=cn-shanghai
...
```

## API授权 {#section_qd5_fx5_ydb .section}

为了确保您的账号安全，建议您使用子账号的身份凭证调用API。如果您使用RAM子账号调用物联网平台API，您需要为该RAM子账号创建、授予相应的授权策略。

为子账号授权调用API，请参见[IoT API 授权映射表](../../../../../intl.zh-CN/用户指南/账号与登录/RAM授权管理/IoT API 授权映射表.md#)。

