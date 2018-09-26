# QueryTopicReverseRouteTable {#reference_yj1_fjd_xdb .reference}

调用该接口查询指定Topic订阅的源Topic，即反向路由表。

## 请求参数 {#section_qdj_wl5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryTopicReverseRouteTable。|
|Topic|String|是|要查询的目标Topic，即接收消息的Topic。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_aw4_dm5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|SrcTopics|List|调用成功时，返回的源Topic列表，即被订阅的Topic列表。|

## 示例 {#section_ok4_4m5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryTopicReverseRouteTable
&Topic=%2Fx7aWKW94bb8%2FtestDataToDataHub%2Fupdate
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true,
    "SrcTopics":["/CXi4***/device1/get","/CXi4***/device4/get"]
}
```

