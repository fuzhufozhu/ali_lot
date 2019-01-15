# QueryDeviceGroupByTags {#reference_jpr_1kv_jgb .reference}

调用该接口根据标签查询设备分组。

## 请求参数 {#section_m3q_ckv_jgb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：QueryDeviceGroupByTags。|
|Tags|List|是|标签列表。Tag由TagKey和TagValue组成。具体请参见下表Tag。-   支持根据TagKey和TagValue查询。
-   也支持只输入TagKey进行查询。

|
|CurrentPage|Integer|否|指定显示查询结果的第几页。默认值是1。|
|PageSize|Integer|否|指定每页显示的记录数。默认值是10。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|TagKey|String|是|分组标签键\(key\)。|
|TagValue|String|否|分组标签值\(value\)。|

## 返回参数 {#section_e15_jlv_jgb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页码。|
|Total|Integer|总记录数。|
|Data|Data|调用成功时，返回分组信息。详情见下表DeviceGroup。|

|名称|类型|描述|
|:-|:-|:-|
|GroupName|String|分组名称。|
|GroupID|String|分组ID。|

## 示例 {#section_lh4_gmv_jgb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=QueryDeviceGroupByTags
&Tag.1.TagKey=group
&Tag.1.TagValue=tag
&CurrentPage=1
&PageSize=10
&公共请求参数
```

**返回示例**

JSON格式

```
{
    "PageCount": 1, 
    "Data": {
        "DeviceGroup": [
            {
                "GroupName": "test11", 
                "GroupId": "Z0ElGF5aqc0thBtW"
            }
        ]
    }, 
    "Page": 1, 
    "PageSize": 10, 
    "RequestId": "9599EE98-1642-4FCD-BFC4-039E458A4693", 
    "Success": true, 
    "Total": 1
}
```

XML格式

```
<?xml version="1.0" encoding="utf-8"?>

<QueryDeviceGroupByTags> 
  <PageCount>1</PageCount>  
  <Data> 
    <DeviceGroup> 
      <GroupName>test11</GroupName>  
      <GroupId>Z0ElGF5aqc0thBtW</GroupId> 
    </DeviceGroup> 
  </Data>  
  <Page>1</Page>  
  <PageSize>10</PageSize>  
  <RequestId>9599EE98-1642-4FCD-BFC4-039E458A4693</RequestId>  
  <Success>true</Success>  
  <Total>1</Total> 
</QueryDeviceGroupByTags>
```

