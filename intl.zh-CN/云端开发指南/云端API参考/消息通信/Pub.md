# Pub {#reference_k3b_yrz_wdb .reference}

调用该接口向指定Topic发布消息。

## 请求参数 {#section_elh_ccb_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：Pub。|
|ProductKey|String|是|要发送消息产品Key。|
|TopicFullName|String|是| 要接收消息的Topic，如`/a1Q5XoY****/device1/user/update`。

 **说明：** 

-   不支持系统Topic。
-   指定Topic的操作权限须为发布或发布和订阅。

 您可以调用[QueryProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/QueryProductTopic.md#)接口查询产品下的Topic类列表，或在设备详情页的Topic列表页签下查看设备的具体Topic。

 |
|MessageContent|String|是|要发送的消息主体。您需要将消息原文转换成二进制数据，并进行Base64编码，从而生成消息主体。|
|Qos|Integer|否| 指定消息的发送方式。取值：

 0：最多发送一次。

 1：最少发送一次。

 如果不传入此参数，则使用默认值0。

 **说明：** QoS=1的消息在物联网平台中最多可以保存7天。物联网平台不保存QoS=0的消息。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_x1z_ndb_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|MessageId|String|成功发送消息后，云端生成的消息ID，用于标识该消息。|

## 示例 {#section_vtd_sdb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=Pub
&ProductKey=a1Q5XoY****
&TopicFullName=%2a1Q5XoY****%2Fdevice1%2Fuser%2Fget
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


