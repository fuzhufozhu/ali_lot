# GetDeviceShadow {#reference_s4y_zrz_wdb .reference}

调用该接口查询指定设备的影子信息。

## 请求参数 {#section_nwb_hjb_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetDeviceShadow。|
|ProductKey|String|是|要查询的设备所隶属的产品Key。|
|DeviceName|String|是|要查询的设备名称。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_sg4_mjb_ydb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|ShadowMessage|String| 调用成功时，返回的设备影子信息。

 **说明：** 根据影子设备的不同状态，查询到的影子信息结构有所差别，详情请参考[设备影子开发](../../../../intl.zh-CN/设备端开发指南/C-SDK/设备影子/设备影子介绍.md#)。

 |

## 示例 {#section_zrq_kkb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=GetDeviceShadow
&ProductKey=al*********
&DeviceName=device1
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
          "Success":true,
          "ShadowMessage":{"method":"update","state":{"desired":{"color":"green"},"reported":"\"},"version":1}
      }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
      <GetDeviceShadowResponse>
          <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
          <Success>true</Success>
          <ShadowMessage>{"method":"update","state":{"desired":{"color":"green"},"reported":"\"},"version":1}</ShadowMessage>
      </GetDeviceShadowResponse>
    ```


