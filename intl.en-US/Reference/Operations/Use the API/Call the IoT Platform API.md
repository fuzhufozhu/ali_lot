# Call the IoT Platform API {#reference_vy5_swc_xdb .reference}

## Request structure {#section_qjd_4t5_ydb .section}

You can call the API provided by IoT Platform by sending HTTP or HTTPS requests.

The request structure is as follows:

```
http://Endpoint/?Action=xx&Parameters
```

Specifically:

-   Endpoint: address where you can access IoT Platform. The following addresses are available:
    -   China \(Shanghai\): `iot.cn-shanghai.aliyuncs.com`
    -   Singapore: `iot.ap-southeast-1.aliyuncs.com`
    -   US \(Silicon Valley\): `iot.us-west-1.aliyuncs.com`
    -   Japan \(Tokyo\): `iot.ap-northeast-1.aliyuncs.com`
    -   Germany \(Frankfurt\): `iot.eu-central-1.aliyuncs.com`
-   Action: operation to be performed. For example, when you can call CreateProduct, the operation creates a product.
-   Parameters: request parameters. Separate each parameter with an ampersand \(&\).

    Request parameters are comprised of common request parameters and parameters customized for the API. Common parameters include the API version number, authentication information, and other information.


The following example describes how to call Pub to publish a message to a specified topic.

**Note:** To assist reading, examples provided in this document are formatted.

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateProduct
? Format=XML
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

## Authorization {#section_qd5_fx5_ydb .section}

To ensure the security of your Alibaba Cloud account, we recommend that you call the API as a RAM user. Before calling the API as a RAM user, you need to create and attach specific authorization policies to the RAM user to allow access to the API.

For more information about API permissions that can be granted to RAM users, see [API permissions](../../../../reseller.en-US/User Guide/Accounts and logon/Resource Access Management (RAM)/API permissions.md#).

## Signature {#section_rny_mz5_ydb .section}

IoT Platform authenticates the symmetric-key encrypted identity of request senders using their AccessKey ID and AccessKey Secret . Alibaba Cloud issues AccessKeys to Alibaba Cloud accounts and RAM users. An AccessKey is a credential similar to a logon password. An AccessKey ID is used to identify the user. An AccessKey Secret is used to encrypt the signature string and is the key that the server uses to authenticate the signature string. AccessKey Secrets must be kept confidential.

When calling RPC API, you need to add the signature to your request using the following format:

`https://endpoint/?SignatureVersion=*1.0*&SignatureMethod=*HMAC-SHA1*&Signature=*CT9X0VtwR86fNWSnsc6v8YGOjuE%3D&SignatureNonce=3ee8c1b8-83d3-44af-a94f-4e0ad82fd6cf*`

The following example shows how to sign the request of calling the Pub API operation. Assuming that the AccessKey ID is `testid`, and the AccessKey Secret is `testsecret`, the original request that is not signed is as follows:

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

To calculate the signature, follow these steps:

1.  Use the request parameters to create the string to sign.

    ```
    GET&%2F&AccessKeyId%3Dtestid%26Action%3DPub%26Format%3DXML%26MessageContent%3DaGVsbG93b3JsZA%253D%26ProductKey%3D12345abcdeZ%26Qos%3D0%26RegionId%3Dcn-shanghai%26ServiceCode%3Diot%26SignatureMethod%3DHMAC-SHA1%26SignatureNonce%3D0715a395-aedf-4a41-bab7-746b43d38d88%26SignatureVersion%3D1.0%26Timestamp%3D2017-10-02T09%253A39%253A41Z%26TopicFullName%3D%252FproductKey%252Ftestdevice%252Fget%26Version%3D2017-04-20
    ```

2.  Compute the HMAC of the string to sign. 

      Append an ampersand \(&\) to the AccessKey Secret to use the new string as the key to compute the HMAC. In this example, the key is `testsecret&`.

    ```
    Y9eWn4nF8QPh3c4zAFkM/k/u7eA=
    ```

3.  Add this signature to your request as the Signature parameter to produce the following request URL:

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


