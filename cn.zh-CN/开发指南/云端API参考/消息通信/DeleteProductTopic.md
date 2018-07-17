# DeleteProductTopic {#reference_qyn_cjd_xdb .reference}

调用该接口删除指定的Topic类。

## 请求参数 {#section_ycv_4g5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteProductTopic。|
|TopicId|Long|是|要删除的Topic类的 ID。|

## 返回参数 {#section_jbl_yg5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

## 示例 {#section_vhq_dh5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteProductTopic
&TopicId=10000
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true
}
```

