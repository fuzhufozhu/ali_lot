# CreateProductTopic {#reference_ilx_vrz_wdb .reference}

调用该接口为指定产品创建产品Topic类。

## 限制说明 {#section_uy5_vyt_xdb .section}

一个产品下最多允许创建50个Topic类。

## 请求参数 {#section_tvj_zyt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateProductTopic。|
|ProductKey|String|是|要为其创建Topic类的产品Key。|
|TopicShortName|String|是| 设置Topic类的自定义类目名称。Topic类默认包含productKey和deviceName两级类目，类目间以正斜线（/）分隔，其格式为：`productKey/deviceName/topicShortName`。

 **说明：** 每级类目的名称只能包含字母、数字和下划线（\_），且不能为空。

 |
|Operation|String|是| 设备对该Topic类的操作权限，取值：

 SUB：订阅。

 PUB：发布。

 ALL：发布和订阅。

 |
|Desc|String|否|Topic类的描述信息。长度限制为100字符（一个汉字占一个字符）。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_hn2_nb5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|TopicId|Long| 调用成功时，IoT Hub为新建的Topic类生成的Topic ID。

 **说明：** 请妥善保管该信息。在调用与该Topic类相关的接口时，您可能需要提供对应的Topic ID。

 |

## 示例 {#section_uqv_wb5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateProductTopic
&ProductKey=al*********
&TopicShortName=submit
&Operation=PUB
&Desc=submit a test topic
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true,
    "TopicId":10000
}
```

