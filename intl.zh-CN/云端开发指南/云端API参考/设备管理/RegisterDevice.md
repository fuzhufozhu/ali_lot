# RegisterDevice {#reference_pc2_kpz_wdb .reference}

调用该接口在指定产品下注册设备。

## 接口说明 {#section_r5z_4wf_xdb .section}

注册设备指在物联网平台产品下添加设备。在指定产品下成功注册设备后，阿里云物联网平台为设备颁发全局唯一的设备ID（IotId），用来标识该设备。在进行与设备相关的操作时，您可能需要提供目标设备的IotId。

**说明：** 除了IotId，您也可以使用ProductKey和DeviceName组合来标识一个设备。其中ProductKey是新建产品时物联网平台为产品颁发的产品Key，DeviceName是注册设备时由您指定或由系统随机生成的设备名称。IotId的优先级高于ProductKey和DeviceName组合。

-   如果您希望在一个产品下批量注册多个设备，并且随机生成设备名，您可以调用[BatchRegisterDevice](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDevice.md#)接口。
-   如果您希望在一个产品下批量注册多个设备，并且为每个设备单独命名，您需要先调用[BatchCheckDeviceNames](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md#)接口为每个设备命名，并生成申请批次ID（ApplyId），再调用[BatchRegisterDeviceWithApplyId](intl.zh-CN/云端开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md#)接口批量注册设备。

## 请求参数 {#section_fkk_fyf_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：RegisterDevice。|
|ProductKey|String|是|待注册设备所隶属的产品的Key，即产品的唯一标识符。|
|DeviceName|String|否| 为待注册的设备命名。设备名称长度为4-32个字符，可以包含英文字母、数字和特殊字符：连字符（-）、下划线（\_）、at符号（@）、点号（.）、和英文冒号（:）。

 DeviceName通常与ProductKey组合使用，用作标识具体的设备。

 **说明：** 如果不传入该参数，则由系统随机生成设备名称。

 |
|Nickname|String|否|为待注册的设备设置备注名称。备注名称长度为4-64个字符，可包含中文汉字、英文字母、数字和下划线（\_）。一个中文汉字算2字符。 **说明：** 如果不传入该参数，系统不会为设备生成备注名称

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_azr_kzf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回注册的设备信息。详情参见下表DeviceInfo。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|设备隶属的产品Key。|
|DeviceName|String|设备名称。 **说明：** 请妥善保管该信息。

 |
|DeviceSecret|String| 设备密钥。

 **说明：** 请妥善保管该信息。

 |
|IotId|String| 物联网平台为该设备颁发的设备ID，作为该设备的唯一标识符。

 **说明：** 请妥善保管该信息。

 |
|Nickname|String|设备的备注名称。 若您没有为该设备设置备注名称，则该参数返回为空。

 |

## 示例 {#section_g52_21g_xdb .section}

**请求示例**

``` {#codeblock_e0o_fi4_1yq}
https://iot.cn-shanghai.aliyuncs.com/?Action=RegisterDevice
&ProductKey=a1rYuVF****
&DeviceName=device1
&Nickname=detectors_in_beijing
&公共请求参数
```

**返回示例**

-   JSON格式

    ``` {#codeblock_vzn_p95_20z}
    {
        "RequestId": "57b144cf-09fc-4916-a272-a62902d5b207", 
        "Success": true, 
        "Data": {
            "DeviceName": "device1", 
            "ProductKey": "a1rYuVF****", 
            "DeviceSecret": "tXHf4ezGEHcwdyMwoCDHGBmk9avi****", 
            "IotId": "CqXL5h5ysRTA4NxjABjj0010fa****", 
            "Nickname": "detectors_in_beijing"
        }
    }
    ```

-   XML格式

    ``` {#codeblock_kce_yf0_ux3}
    <?xml version='1.0' encoding='utf-8'?>
    <RegisterDeviceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <DeviceName>device1</DeviceName>
            <ProductKey>a1rYuVF****</ProductKey>
            <DeviceSecret>tXHf4ezGEHcwdyMwoCDHGBmk9avi****</DeviceSecret>
            <IotId>CqXL5h5ysRTA4NxjABjj0010fa****</IotId>
            <Nickname>detectors_in_beijing</Nickname>
        </Data>
    </RegisterDeviceResponse>
    ```


