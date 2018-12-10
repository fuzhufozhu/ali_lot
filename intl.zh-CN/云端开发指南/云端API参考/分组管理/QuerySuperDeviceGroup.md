# QuerySuperDeviceGroup {#reference_xxb_ss4_1gb .reference}

调用该接口根据子分组ID查询父分组信息。

## 请求参数 {#section_itm_ws4_1gb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QuerySuperDeviceGroup。|
|GroupId|String|是|子分组ID，分组的全局唯一标识符。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_t2p_ht4_1gb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的父分组信息数据。请参考下表GroupInfo。|

|名称|类型|描述|
|:-|:-|:-|
|GroupName|String|子分组所属的父分组名称。|
|GroupId|String|子分组所属的父分组ID。|
|GroupDesc|String|父分组描述。|

## 示例 {#section_v2d_154_1gb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=QuerySuperDeviceGroup
&GroupId=DMoI2Kby5m62Sirz
&公共请求参数
```

**返回示例**

JSON 格式

```
{
    "Data":{
        "GroupName":"IOTTEST",
        "GroupId":"tDQvBJqbUyHskDse",
	"GroupDesc":"A test."
    },
    "RequestId":"7411716B-A488-4EEB-9AA0-6DB05AD2491F",
    "Success":true
}
```

XML格式

```
<?xml version="1.0" encoding="UTF-8" ?>
<QuerySuperDeviceGroupResponse>
	<Data>
	    <GroupName>IOTTEST</GroupName>
	    <GroupId>tDQvBJqbUyHskDse</GroupId>
	    <GroupDesc>A test.</GroupDesc>
	</Data>
	<RequestId>7411716B-A488-4EEB-9AA0-6DB05AD2491F</RequestId>
	<Success>true</Success>
</QuerySuperDeviceGroupResponse>
```

