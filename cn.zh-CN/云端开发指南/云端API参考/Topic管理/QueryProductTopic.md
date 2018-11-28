# QueryProductTopic {#reference_l5f_x3d_xdb .reference}

调用该接口查询指定产品的Topic类。

## 请求参数 {#section_abq_1f5_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryProductTopic。|
|ProductKey|String|是|要查询Topic类的产品Key。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_hmz_hf5_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|ProductTopicInfo|调用成功时，返回的Topic类信息列表。详情参见[ProductTopicInfo](#table_q1v_mf5_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|产品Key。|
|Id|String|Topic类的 ID。|
|TopicShortName|String|Topic类中除productKey和deviceName以外的类目。|
|Operation|String| 设备对该Topic类的操作权限，取值：

 0：发布

 1：订阅

 2：发布和订阅

 |
|Desc|String|Topic类的描述信息。|

## 示例 {#section_fxh_zf5_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryProductTopic
&ProductKey=HMyB*******
&公共请求参数
```

**返回示例**

```
{
    "Data": [{
        "Id": "10000",
        "Operation": "0",
        "ProductKey": "HMyB***",
        "TopicShortName": "/HMyB***/${deviceName}/update"
    },
    {
        "Id": "10001",
        "Operation": "2",
        "ProductKey": "HMyB***",
        "TopicShortName": "/HMyB***/${deviceName}/update/error"
    },
    {
        "Id": "10002",
        "Operation": "1",
        "ProductKey": "HMyB***",
        "TopicShortName": "/HMyB***/${deviceName}/get"
    }],
    "RequestId": "B953EAFF-CFF6-4FF8-BC94-8B89F018E4DD",
    "Success": true
}
```

