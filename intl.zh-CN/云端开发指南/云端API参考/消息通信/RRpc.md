# RRpc {#reference_vgv_ckd_xdb .reference}

调用该接口向指定设备发送请求消息，并同步返回响应。

## 请求参数 {#section_wws_dhb_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：RRpc。|
|ProductKey|String|是|要发送消息的产品Key。|
|DeviceName|String|是|要接收消息的设备名称。|
|RequestBase64Byte|String|是|要发送的请求消息内容经过Base64编码得到的字符串格式数据。|
|Timeout|Integer|是|等待设备回复消息的时间，单位是毫秒，取值范围是1,000 ~5,000。|
|Topic|String|否|使用自定义的RRPC相关Topic。需要设备端配合使用，请参见设备端开发[自定义Topic](../../../../../intl.zh-CN/用户指南/RRPC/自定义Topic.md#)。不传入此参数，则使用系统默认的RRPC Topic。

|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_wbr_qhb_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|MessageId|String|成功发送请求消息后，云端生成的消息ID，用于标识该消息。|
|RrpcCode|String| 调用成功时，生成的调用返回码，标识请求状态。取值：

 UNKNOW：系统异常

 SUCCESS：成功

 TIMEOUT：设备响应超时

 OFFLINE：设备离线

 HALFCONN：设备离线（设备连接断开，但是断开时间未超过一个心跳周期）

 |
|PayloadBase64Byte|String|设备返回结果Base64编码后的值。|

## 示例 {#section_g4c_33b_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=RRpc
&ProductKey=al*******
&DeviceName=device1
&RequestBase64Byte=aGVsbG8gd29ybGQ%3D
&TimeOut=1000
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          "RequestId":"41C4265E-F05D-4E2E-AB09-E031F501AF7F",
          "Success":true,
          "RrpcCode":"SUCCESS",
          "PayloadBase64Byte":"d29ybGQgaGVsbG8="
          "MessageId":889455942124347392
      }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
      <RRpcResponse>
          <RequestId>41C4265E-F05D-4E2E-AB09-E031F501AF7F<RequestId/>
          <Success>true</Success>
          <RrpcCode>SUCCESS</RrpcCode>
          <PayloadBase64Byte>d29ybGQgaGVsbG8=</PayloadBase64Byte>
          <MessageId>889455942124347392</MessageId>
      </RRpcResponse>
    ```


