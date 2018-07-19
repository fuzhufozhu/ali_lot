# RegisterDevice {#reference_pc2_kpz_wdb .reference}

用该接口在指定产品下注册设备。

## 接口说明 {#section_r5z_4wf_xdb .section}

注册设备指在产品下新建设备。在指定产品下成功注册设备后，阿里云IoT平台为注册的设备颁发全局唯一的设备ID（IotId），用来标识该设备。在进行与设备相关的操作时，您可能需要提供目标设备的IotId。

**说明：** 除了IotId，您也可以使用ProductKey和DeviceName组合来标识一个设备。其中ProductKey是新建产品时IoT平台为产品颁发的产品Key，DeviceName是注册设备时由您指定或由系统随机生成的设备名称。IotId的优先级高于ProductKey和DeviceName组合。

-   如果您希望在一个产品下批量注册多个设备，并且随机生成设备名，您可以调用BatchRegisterDevice接口。请参见[BatchRegisterDevice](cn.zh-CN/开发指南/云端API参考/设备管理/BatchRegisterDevice.md#)
-   如果您希望在一个产品下批量注册多个设备，并且为每个设备单独命名，您需要先调用BatchCheckDeviceNames接口为每个设备命名，并生成申请批次ID（ApplyId），再调用BatchRegisterDeviceWithApplyId接口批量注册设备。请参见[BatchCheckDeviceNames](cn.zh-CN/开发指南/云端API参考/设备管理/BatchCheckDeviceNames.md#)和[BatchRegisterDeviceWithApplyId](cn.zh-CN/开发指南/云端API参考/设备管理/BatchRegisterDeviceWithApplyId.md#)。

## 请求参数 {#section_fkk_fyf_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：RegisterDevice。|
|ProductKey|String|是|指定要为其注册设备的产品的Key。|
|DeviceName|String|否| 为要注册的设备命名。设备名称应包含4-32个字符，可以包含英文字母、数字和特殊字符（连字符、下划线、@符号、点号、和英文冒号）。

 DeviceName通常与ProductKey组合使用，用作标识具体的设备。

 **说明：** 如果不传入该参数，则由系统随机生成设备名称。

 |

## 返回参数 {#section_azr_kzf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|DeviceInfo|调用成功时，返回注册的设备信息。详情参见[DeviceInfo](#table_iyr_szf_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|ProductKey|String|设备隶属的产品Key。|
|DeviceName|String|设备名称。**说明：** 请妥善保管该信息。

|
|DeviceSecret|String| 设备密钥。

 **说明：** 请妥善保管该信息。

 |
|IotId|String| IoT平台为该设备颁发的设备ID，作为该设备的唯一标识符。

 **说明：** 请妥善保管该信息。

 |

## 示例 {#section_g52_21g_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=RegisterDevice
&ProductKey=al*********
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
        "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
        "Success": true，
        "Data": {
            "DeviceName": "CqXL5h5ysRTA4NxjABjj",
            "ProductKey": "a1*********",
            "DeviceSecret": "tXHf4ezGEHcwdyMwoCDHGBmk9avi****",
            "IotId": "CqXL5h5ysRTA4NxjABjj0010fa****"
        }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <RegisterDeviceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <DeviceName>CqXL5h5ysRTA4Nxj****</DeviceName>
            <ProductKey>a1rYuVF****</ProductKey>
            <DeviceSecret>tXHf4ezGEHcwdyMwoCDHGBmk9avi****</DeviceSecret>
            <IotId>CqXL5h5ysRTA4NxjABjj0010fa****</IotId>
        </Data>
    </RegisterDeviceResponse>
    ```


