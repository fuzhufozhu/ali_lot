# BatchAddDeviceGroupRelations {#reference_lk3_rjx_kfb .reference}

调用该接口添加设备到某一分组（可批量添加设备）。

## 请求参数 {#section_rw4_dd2_lfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作。取值：BatchAddDeviceGroupRelations。|
|GroupId|String|是|分组ID，分组的全局唯一标识符。|
|Devices|List<Device\>|是|要添加到分组的设备信息，请参见下表Device。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|ProductKey|String|是|要添加到分组的设备所属的产品Key。|
|DeviceName|String|是|要添加到分组的设备名称。|

## 返回参数 {#section_kqs_lf2_lfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|返回的结果信息。|
|Code|String|返回错误码，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|ValidDeviceCount|Integer|请求参数中合法的设备数量。|
|SuccessAddedDeviceCount|Integer|成功添加到分组的设备数量。|
|ExceedMaxGroupDeviceCount|Integer|请求参数中，已经添加到10个或者10个以上分组的设备数量（一个设备最多添加到10个分组）。|
|AlreadyRelatedGroupDeviceCount|Integer|原已经添加到此分组的设备数量。|

## 示例 {#section_cdj_2mf_lfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?Action=BatchAddDeviceGroupRelations
&Device.1.DeviceName=ZHuPo6sZzv7pOzYhv31y
&Device.1.ProductKey=a1kORrKiwQj
&Device.2.DeviceName=rB4V9PDW2FCPmwuf2N3Y
&Device.2.ProductKey=a1kORrKiwQj
&GroupId=6VfhebLg5iUerXep
&公共请求参数
```

**返回示例**

```
{
    "SuccessAddedDeviceCount":2,
    "ExceedTenGroupDeviceCount":0,
    "ErrorMessage":"2 devices have been added, and 0 devices failed to be added.",
    "ValidDeviceCount":2,
    "AlreadyRelatedGroupDeviceCount":0,
    "RequestId":"671D0F8F-FDC7-4B12-93FA-336C079C965A",
    "Success":true   
}
```

