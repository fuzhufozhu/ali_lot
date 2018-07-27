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
?Format=XML
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

为子账号授权调用API，请参见[IoT API 授权映射表](../../../../intl.zh-CN/用户指南/账号与登录/RAM授权管理/IoT API 授权映射表.md#)。

## API签名 {#section_rny_mz5_ydb .section}

物联网平台会对每个API请求进行身份验证，无论使用HTTP还是HTTPS协议提交请求，都需要在请求中包含签名（Signature）信息。详情参见[RPC API签名](https://www.alibabacloud.com/help/zh/doc-detail/66384.htm)。

物联网平台通过使用AccessKey ID和AccessKey Secret进行对称加密的方法来验证请求的发送者身份。AccessKey是为阿里云账号和RAM用户发布的一种身份凭证\(类似于用户的登录密码\)，其中AccessKey ID用于标识访问者的身份，AccessKey Secret是用于加密签名字符串和服务器端验证签名字符串的密钥，必须严格保密。

RPC API需按如下格式在请求中增加签名（Signature）：

`https://endpoint/?SignatureVersion=*1.0*&SignatureMethod=*HMAC-SHA1*&Signature=*CT9X0VtwR86fNWSnsc6v8YGOjuE%3D&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf*`

以调用Pub接口为例。假设AccessKey ID是`testid`， AccessKey Secret是`testsecret`，则签名前的请求URL如下：

```
http://iot.cn-shanghai.aliyuncs.com/?Action=Pub
&MessageContent=aGVsbG93b3JsZA%3D
&Timestamp=2017-10-02T09%3A39%3A41Z
&SignatureVersion=1.0
&ServiceCode=iot
&Format=XML
&Qos=0
&SignatureNonce=0715a395-aedf-4a41-bab7-746b43d38d88
&Version=2017-04-20
&AccessKeyId=testid
&SignatureMethod=HMAC-SHA1
&RegionId=cn-shanghai
&ProductKey=12345abcdeZ
&TopicFullName=%2FproductKey%2Ftestdevice%2Fget
```

签名计算步骤：

1.  使用请求参数创建待签名字符串：

    ```
    GET&%2F&AccessKeyId%3Dtestid%26Action%3DPub%26Format%3DXML%26MessageContent%3DaGVsbG93b3JsZA%253D%26ProductKey%3D12345abcdeZ%26Qos%3D0%26RegionId%3Dcn-shanghai%26ServiceCode%3Diot%26SignatureMethod%3DHMAC-SHA1%26SignatureNonce%3D0715a395-aedf-4a41-bab7-746b43d38d88%26SignatureVersion%3D1.0%26Timestamp%3D2017-10-02T09%253A39%253A41Z%26TopicFullName%3D%252FproductKey%252Ftestdevice%252Fget%26Version%3D2017-04-20
    ```

2.  计算待签名的HMAC的值。

    在AccessKey Secret后添加一个“&”作为计算HMAC值的key。本示例中的key为`testsecret&`。

    ```
    Y9eWn4nF8QPh3c4zAFkM/k/u7eA=
    ```

3.  将签名作为Signature参数加入到URL请求中，最后得到的URL为：

    ```
    http://iot.cn-shanghai.aliyuncs.com/?Action=Pub
    &MessageContent=aGVsbG93b3JsZA%3D
    &Timestamp=2017-10-02T09%3A39%3A41Z
    &SignatureVersion=1.0
    &ServiceCode=iot
    &Format=XML
    &Qos=0
    &SignatureNonce=0715a395-aedf-4a41-bab7-746b43d38d88
    &Version=2017-04-20
    &AccessKeyId=testid
    &Signature=Y9eWn4nF8QPh3c4zAFkM%2Fk%2Fu7eA%3D
    &SignatureMethod=HMAC-SHA1
    &RegionId=cn-shanghai
    &ProductKey=12345abcdeZ
    &TopicFullName=%2FproductKey%2Ftestdevice%2Fget
    ```


