# CreateTopicRouteTable {#reference_tp3_djd_xdb .reference}

调用该接口新建Topic间的消息路由关系。

## 请求参数 {#section_dwz_ph5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateTopicRouteTable。|
|SrcTopic|String|是|源Topic，即被订阅的Topic。**说明：** 不支持以 sys 开头的系统 Topic。

|
|DstTopic|List|是|目标Topic列表，即从SrcTopic订阅消息的Topic列表。\(即使只有一个Topic，也使用数组格式。）**说明：** 不支持以 sys 开头的系统 Topic。

|

## 返回参数 {#section_lm4_335_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|FailureTopics|List|未能成功创建路由关系的Topic列表。|

## 示例 {#section_gn2_435_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateTopicRouteTable
&SrcTopic=/CXi4***/device1/get
&DstTopic=[{/CXi4***/device2/get},{/CXi4***/device3/get}]
&公共请求参数
```

**返回参数**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true,
    "FailureTopics":[]
}
```

