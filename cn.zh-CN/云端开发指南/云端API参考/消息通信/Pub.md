# Pub {#reference_k3b_yrz_wdb .reference}

调用该接口向指定Topic发布消息。

## 请求参数 {#section_elh_ccb_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：Pub。|
|ProductKey|String|是|要发送消息产品ID。|
|TopicFullName|String|是| 要接收消息的Topic全称。可以是用户Topic类和您自定义的Topic类（不支持系统Topic类）。

 -   用户Topic类结构：`/productKey/deviceName/update`

-   自定义Topic类结构：`productKey/deviceName/topicShortName`

其中，topicShortName是您创建Topic时自定义的类目。


 您可以调用QueryProductTopic接口查询产品下所有的Topic列表。

 |
|MessageContent|String|是|要发送的消息主体。您需要将消息原文转换成二进制数据，并进行Base64编码，从而生成消息主体。|
|Qos|Integer|否| 指定消息的发送方式。取值：

 0：最多发送一次。

 1：最少发送一次。

 如果不传入此参数，则使用默认值0。

 **说明：** 一条消息在物联网平台中最多可以保存7天。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_x1z_ndb_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|MessageId|String|成功发送消息后，云端生成的消息ID，用于标识该消息。|

## 示例 {#section_vtd_sdb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=Pub
&ProductKey=al*********
&TopicFullName=%2FproductKey%2FdeviceName%2Fget
&MessageContent=aGVsbG8gd29ybGQ%3D
&Qos=0
&公共请求参数
```

**返回示例**

-   JSON 格式

    ```
    {
          "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
          "Success":true,
          "MessageId":889455942124347329
      }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?> 
       <PubResponse>
          <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
          <Success>true</Success>
          <MessageId>889455942124347329</MessageId>
      </PubResponse>
    ```


