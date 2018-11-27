# QueryDeviceGroupByDevice {#reference_uv5_vjv_xfb .reference}

调用该接口查询某一设备所在的分组列表。

## 说明 {#section_its_1kv_xfb .section}

目前一个设备最多能被添加到10个分组中 。

## 请求参数 {#section_s2g_fkv_xfb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceGroupByDevice。|
|ProductKey|String|是|产品Key。|
|DeviceName|String|是|设备名称。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)|

## 返回参数 {#section_hgw_4lv_xfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|Code|String|调用失败时，返回的错误码。请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|ErrorMessage|String|调用失败时，返回的错误信息。|
|GroupInfos|List<GroupInfo\>|调用成功时，返回的分组信息。详情请参见下表GroupInfo。|

|名称|类型|描述|
|:-|:-|:-|
|GroupId|String|分组ID。|
|GroupName|String|分组名称。|
|UtcCreate|String|分组的创建时间。|
|GroupDesc|String|分组描述。|

## 示例 {#section_qxz_zmv_xfb .section}

**请求示例**

```
http://iot.cn-shanghai.aliyuncs.com/?&Action=QueryDeviceGroupByDevice
&DeviceName=test456
&ProductKey=a1SKk9K****
&公共请求参数
```

**返回示例**

```
{
  "RequestId": "7941C8CD-7764-4A94-8CD9-E2762D4A73AC",
  "GroupInfos": {
    "GroupInfo": [
      {
        "GroupDesc": "father desc",
        "GroupName": "father1543152336554",
        "UtcCreate": "2018-11-25T13:25:37.000Z",
        "GroupId": "6a3FF2XE2BKayWsM"
      }
    ]
  },
  "Success": true
}
```

