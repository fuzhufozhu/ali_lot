# UpdateRule {#reference_szx_zjd_xdb .reference}

调用该接口修改指定的规则。

## 请求参数 {#section_yrn_rx1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：UpdateRule。|
|RuleId|Long|是|要修改的规则ID。|
|Name|String|是|规则名称。支持使用中英文字符、数字和下划线（\_），长度应为4~30位（一个中文字符算2位）。|
|ProductKey|String|否|应用该规则的产品Key。|
|ShortTopic|String|否| 应用该规则的具体Topic，格式为：`${deviceName}/topicShortName`。其中，$\{deviceName\}指具体设备的名称，topicShortName是自定义类目。

 调用[QueryDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevice.md#)接口，查看产品下的所有设备。返回结果中包含所有的DeviceName。

 调用[QueryProductTopic](intl.zh-CN/云端开发指南/云端API参考/Topic管理/UpdateProductTopic.md#)接口，查看产品下的所有Topic类。返回结果中包含所有的TopicShortName。

 示例：

 -   系统Topic的ShortTopic，如：`${deviceName}/thing/event/property/post`，其中`${deviceName}`可以使用通配符`+`代替，表示产品下所有设备名称。

-   自定义Topic的ShortTopic，如：`${deviceName}/user/get`。

指定自定义Topic时，可以使用通配符`+`和`#`。

    -   `${deviceName}`可以使用通配符`+`代替，表示产品下所有设备；
    -   之后字段可以用`/user/#`，`#`表示`/user`层级之后的所有层级名称。

使用通配符，请参见[Topic类中的通配符](../../../../../intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。

-   设备状态Topic的ShortTopic：`${deviceName}`。

 |
|Select|String|否| 要执行的SQL Select语句。具体内容参照SQL表达式。

 **说明：** 此处传入的是Select下的内容。例如，如果Selcet语句为`Select a,b,c`，则此处传入`a,b,c`。

 |
|RuleDesc|String|否|规则的描述信息。长度限制为100字符（一个汉字占一个字符）。|
|Where|String|否| 规则的触发条件。具体内容参照SQL表达式。

 **说明：** 此处传入的是Where中的内容。例如，如果Where语句为`Where a>10`，则此处传入`a>10`。

 |
|TopicType|Integer|否| -   0：系统Topic，包含：

    -   `/thing/event/property/post` 设备上报的属性消息。
    -   `/thing/event/${tsl.event.identifier}/post`，`${}`中的是产品物模型中事件identifier。设备上报的事件消息
    -   `thing/lifecycle` 设备生命周期变更消息。
    -   `/thing/downlink/reply/message`设备响应云端指令的结果消息。
    -   `/thing/list/found`网关上报发现子设备消息。
    -   `thing/topo/lifecycle`设备拓扑关系变更消息。
-   1：自定义Topic。

-   2：设备状态消息Topic：`/as/mqtt/status/${productKey}/${deviceName}`。


 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_qrf_gy1_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_p4w_ky1_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateRule
&RuleId=1000
&Name=test_2
&ProductKey=al*********
&ShortTopic=+/get
&Select=a,b,c
&RuleDesc=test
&Where=a>10
&TopicType=1
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"9A2F243E-17FE-4846-BAB5-D02A25155AC4",
    "Success":true
}
```

