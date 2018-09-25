# UpdateProductTopic {#reference_eyg_w3d_xdb .reference}

调用该接口修改指定的产品Topic类。

## 请求参数 {#section_prr_md5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：UpdateProductTopic。|
|TopicId|Long|是|要修改的Topic类的 ID。|
|TopicShortName|String|否| 设置Topic类的自定义类目名称。Topic类默认包含productKey和deviceName两级类目，类目间以正斜线（/）分隔，其格式为：`productKey/deviceName/topicShortName`。

 **说明：** 每级类目的名称只能包含字母、数字和下划线（\_），且不能为空。

 |
|Operation|String|否| 设备对该Topic类的操作权限，取值：

 SUB：订阅。

 PUB：发布。

 ALL：发布和订阅。

 |
|Desc|String|否|Topic类的描述信息。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回示例 {#section_hpq_b25_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|

## 示例 {#section_ezk_m25_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateProductTopic
&ProductKey=al*********
&TopicId=10000
&TopicShortName=resubmit
&Operation=PUB
&Desc=resubmit a test topic
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true
}
```

