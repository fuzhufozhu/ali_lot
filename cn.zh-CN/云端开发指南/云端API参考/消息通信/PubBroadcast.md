# PubBroadcast {#reference_i4y_bkd_xdb .reference}

调用该接口向指定产品对应的所有Topic发布广播消息。

## 请求参数 {#section_idz_y2b_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：PubBroadcast。|
|ProductKey|String|是|要发送广播消息的产品ID。|
|TopicFullName|String|是| 要接收广播消息的Topic全称。格式为：`/broadcast/productKey/自定义字段`。其中，productKey是要接收广播消息的具体产品Key，自定义字段中您可以指定任意字段。

 一个广播Topic最多可被1,000个设备订阅。

 如果您的设备超过数量限制，您可以对设备进行分组。例如，如果您有5,000个设备，您可以将设备按每组1,000个，而分成5组。您需要分5次调用广播Topic，自定义字段分别设置为group1/2/3/4/5，然后让每组设备分别订阅各自分组的广播Topic。

 |
|MessageContent|String|是|要发送的消息主体。您需要将消息原文转换成二进制数据，并进行Base64编码，从而生成消息主体。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_mly_bgb_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|MessageId|String|成功发送消息后，云端生成的消息ID，用于标识该消息。|

## 示例 {#section_wjc_fgb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=PubBroadcast
&ProductKey=al**********
&TopicFullName=%252Fbroadcast%252FUPqSxj2vXXX%252Fxxx
&MessageContent=aGVsbG93b3JsZA==
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
          "Success":true,
      }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
      <PubBroadcastResponse>
          <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
          <Success>true</Success>
      </PubBroadcastResponse>
    ```


