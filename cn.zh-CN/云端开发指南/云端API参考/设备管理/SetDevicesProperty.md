# SetDevicesProperty {#reference_qmz_ctj_sfb .reference}

调用该接口批量设置设备属性值。

## 使用限制 {#section_f1v_4vj_sfb .section}

单账号的每秒请求数最大限制为10 QPS。

**说明：** 子账号共享主账号配额。

## 请求参数 {#section_jgt_v2t_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：SetDevicesProperty。|
|ProductKey|String|是| 要设置属性值的设备所隶属的产品Key。

 |
|DeviceNames|List<String\>|是| 要设置属性值的设备名称列表。目前，最多支持100个设备。

 |
|Items|String|是| 要设置的属性信息，组成为key:value，数据格式为JSON String。

 Items数据详情，参见下表Items。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String| 要设置的属性的标识符（identifier）。设备的属性Identifier，可在控制台中设备所属的产品的功能定义中查看。

 **说明：** 设置的属性必需是读写型。如果您指定了一个只读型的属性，设置将会失败。

 |
|value|Obejct|属性值。取值需和您定义的属性的数据类型和取值范围保持一致。|

## 返回参数 {#section_nry_z5j_sfb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_vmf_1wj_sfb .section}

**请求示例**

``` {#codeblock_6ee_8za_fbf}
https://iot.cn-shanghai.aliyuncs.com/&Action=SetDevicesProperty
&DeviceName.1=1102andriod02
&DeviceName.2=1102android01
&Items=%7B%20%20%20%20%20%22Data%22%3A%221372060916%22%2C%20%20%20%20%20%22Status%22%3A1%20%7D
&ProductKey=a1hWjHDWUbF
&公共请求参数
```

**返回示例**

JSON 格式

``` {#codeblock_et9_fwi_c3c}
{
  "RequestId": "2E19BDAF-0FD0-4608-9F41-82D230CFEE38",
  "Success": true
}
```

XML 格式

``` {#codeblock_676_242_buf}
<?xml version='1.0' encoding='utf-8'?>
<SetDevicesPropertyResponse>
    <RequestId>"2E19BDAF-0FD0-4608-9F41-82D230CFEE3</RequestId>
    <Success>true</Success>
</SetDevicesPropertyResponse>
```

