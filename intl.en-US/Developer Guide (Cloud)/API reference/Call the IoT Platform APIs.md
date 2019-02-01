# Call the IoT Platform APIs {#reference_vy5_swc_xdb .reference}

## Request structure {#section_qjd_4t5_ydb .section}

You can call the API provided by IoT Platform by sending HTTP or HTTPS requests.

The request structure is as follows:

```
http://Endpoint/?Action=xx&Parameters
```

In the request,

-   Endpoint: address where you can access IoT Platform. The following addresses are available:
    -   China \(Shanghai\): `iot.cn-shanghai.aliyuncs.com`
    -   Singapore: `iot.ap-southeast-1.aliyuncs.com`
    -   US \(Silicon Valley\): `iot.us-west-1.aliyuncs.com`
    -   Japan \(Tokyo\): `iot.ap-northeast-1.aliyuncs.com`
    -   Germany \(Frankfurt\): `iot.eu-central-1.aliyuncs.com`
-   Action: operation to be performed. For example, call the Pub operation to publish messages.
-   Parameters: request parameters. Separate each parameter with an ampersand \(&\).

    Request parameters are comprised of common request parameters and parameters customized for the API. Common parameters include the API version number, authentication information, and other information.


The following example describes how to call Pub to publish a message to a specified topic.

**Note:** In the following example, the service region is China \(Shanghai\). To assist reading, examples provided in this document are formatted.

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

## API authorization {#section_qd5_fx5_ydb .section}

To ensure the security of your Alibaba Cloud account, we recommend that you call the API as a RAM user. Before calling the API as a RAM user, you need to create and attach specific authorization policies to the RAM user to allow access to the API.

For more information about API permissions that can be granted to RAM users, see [API permissions](../../../../../reseller.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/API permissions.md#).

