# CreateTopicRouteTable {#reference_tp3_djd_xdb .reference}

调用该接口新建Topic间的消息路由关系。

## 限制说明 {#section_llf_xzn_t2b .section}

一个源Topic最多可对应100个目标Topic。

## 请求参数 {#section_dwz_ph5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateTopicRouteTable。|
|SrcTopic|String|是|源Topic，即被订阅的Topic。如，`SrcTopic=/x7aWKW9****/testDataToDataHub/update`。**说明：** 不支持以 sys 开头的系统 Topic。

|
|DstTopic|List|是|目标Topic列表，即从SrcTopic订阅消息的Topic列表。即使只有一个Topic，也使用数组格式。如，`DstTopic.1=/x7aWKW9****/deviceNameTest1/add`，`DstTopic.2=/x7aWKW9****/deviceNameTest2/delete`。**说明：** 不支持以 sys 开头的系统 Topic。

|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_lm4_335_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|FailureTopics|List|未能成功创建路由关系的Topic列表。|

## 示例 {#section_gn2_435_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateTopicRouteTable
&SrcTopic=%2Fx7aWKW9****%2FtestDataToDataHub%2Fupdate
&DstTopic.1=%2Fx7aWKW9****%2FdeviceNameTest1%2Fadd
&DstTopic.2=%2Fx7aWKW9****%2FdeviceNameTest2%2Fdelete
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

