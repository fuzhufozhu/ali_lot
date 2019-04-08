# BatchUpdateDeviceNickname {#reference_d2g_smb_fhb .reference}

调用该接口批量修改设备备注名称。

## 请求参数 {#section_fkk_fyf_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchUpdateDeviceNickname。|
|DeviceNicknameInfo|String|是|包含设备标识参数（ProductKey和DeviceName组合或IotId）和备注名称参数（Nickname）。**说明：** 

-   设备标识信息为必传参数，用于指定设备。
-   Nickname为非必填参数。若不传入，则删除该设备的原有备注名称。

详情请见下表DeviceNicknameInfo。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|IotId|String|否| 要修改别名的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，和ProductKey与DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 指定要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Nickname|String|否|新的设备备注名称。备注名称长度为4-32个字符，可包含中文汉字、英文字母、数字和下划线。一个中文汉字算2字符。**说明：** 若不传入该参数，则删除该设备的原有备注名称。

|

## 返回参数 {#section_cqf_l2m_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_jgy_rrb_fhb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchUpdateDeviceNickname
&DeviceNicknameInfo.1.ProductKey=a1rYuVF****
&DeviceNicknameInfo.1.DeviceName=SR8FiTu1R9tlUR2V1bmi
&DeviceNicknameInfo.1.Nickname=airconditioning_type1
&DeviceNicknameInfo.2.ProductKey=a1yrZMH****
&DeviceNicknameInfo.2.DeviceName=RkQ8CFtNpDok4BEunymt
&DeviceNicknameInfo.2.Nickname=airconditioning_type2
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?>
    <BatchUpdateDeviceNicknameResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
    </BatchUpdateDeviceNicknameResponse>
    ```


