# QueryTopicRouteTable {#reference_n4w_fjd_xdb .reference}

调用该接口查询向指定Topic订阅消息的目标Topic，即指定Topic的路由表。该接口只支持查询用户的Topic。

## 请求参数 {#section_atq_ln5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryTopicRouteTable。|
|Topic|String|是|要查询的源Topic，即发布消息的Topic。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_rsm_sn5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|DstTopics|List|调用成功时，返回的目标Topic列表，即向源Topic订阅消息的Topic列表。|

## 示例 {#section_y2t_yn5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryTopicRouteTable
&Topic=%2Fx7aWKW94bb8%2FtestDataToDataHub%2Fupdate
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"FCC27691-9151-4B93-9622-9C90F30542EC",
    "Success":true,
    "DstTopics":["/CXi4***/device2/get","/CXi4***/device3/get"]
}
```

