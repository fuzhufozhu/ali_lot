# DeleteTopicRouteTable {#reference_vs2_2jd_xdb .reference}

调用该接口删除指定的Topic路由关系。

## 请求参数 {#section_avh_1l5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：DeleteTopicRouteTable。|
|SrcTopic|String|是|源Topic，即被订阅的Topic。如，`SrcTopic.1=/x7aWKW9****/testDataToDataHub/update`。|
|DstTopic|List|是|目标Topic列表，即从SrcTopic订阅消息的Topic列表。如，`DstTopic.1=/x7aWKW9****/deviceNameTest1/add`，`DstTopic.2=/x7aWKW9****/deviceNameTest2/delete`。|
|公共请求参数|-|是|请参见[签名机制](intl.zh-CN/云端开发指南/云端API参考/签名机制.md#)。|

## 返回参数 {#section_njn_hl5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|FailureTopics|List|未能成功删除路由关系的Topic列表。|

## 示例 {#section_mf5_ll5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=DeleteTopicRouteTable
&SrcTopic=%2Fx7aWKW94bb8%2FtestDataToDataHub%2Fupdate
&DstTopic.1=%2Fx7aWKW94bb8%2FdeviceNameTest1%2Fadd
&DstTopic.2=%2Fx7aWKW94bb8%2FdeviceNameTest2%2Fdelete
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true,
    "FailureTopics":[]
}
```

