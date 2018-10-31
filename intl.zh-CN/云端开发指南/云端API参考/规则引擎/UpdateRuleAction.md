# UpdateRuleAction {#reference_zjs_1kd_xdb .reference}

调用该接口修改指定的规则动作。

## 请求参数 {#section_rl5_nz1_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：UpdateRuleAction。|
|ActionId|Long|是|要修改的规则动作ID。|
|Type|String|是| 规则动作类型，取值：

 MNS：将根据规则处理后的Topic数据发送至阿里云消息服务（Message Service）中，进行消息传输。

 FC：将根据规则处理后的Topic数据发送至阿里云函数计算服务，进行事件计算。

 OTS：将根据规则处理后的Topic数据发送至阿里云表格存储，进行NoSQL数据存储。

 REPUBLISH：将根据规则处理后的Topic数据转发至另一个IoT Topic。

 **说明：** 

-   新加坡地域服务（接入endpoint为ap-southeast）不支持FC。
-   美国西部1地域服务（接入endpoint为us-west-1）不支持、FC、。
-   二进制数据格式的规则（即规则的DataType参数是BINARY）不支持OTS。

 |
|Configuration|String|是|该规则动作的配置信息。不同规则动作类型所需配置内容不同。具体要求，请参见[CreateActionRule](intl.zh-CN/云端开发指南/云端API参考/规则引擎/CreateRuleAction.md#section_ev2_m5z_xdb)中的各类型的Configuration描述。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_ctr_z1b_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_g1c_dbb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=UpdateRuleAction
&ActionId=10003
&Type=REPUBLISH
&Configuration={"topic":"/a1********/device5/get","topicType":1}
&公共请求参数
```

**返回示例**

```
{
    "RequestId": "21D327AF-A7DE-4E59-B5D1-ACAC8C024555",
    "Success": true
}
```

