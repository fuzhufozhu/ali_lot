# UpdateDeviceShadow {#reference_yc1_2kd_xdb .reference}

调用该接口修改指定设备的影子信息。

## 请求参数 {#section_ydm_wkb_ydb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：UpdateDeviceShadow。|
|ProductKey|String|是|要修改影子信息的设备所隶属的产品Key。|
|DeviceName|String|是|要修改影子信息的设备名称。|
|ShadowMessage|String|是| 修改后的设备影子信息。示例如下：

 ```
{
"method": "update",
"state": {
"desired": {
"color": "green"
}
},
  "version": 2
}
```

 详情参见 [ShadowMessage](intl.zh-CN/云端开发指南/云端API参考/设备影子/UpdateDeviceShadow.md#table_ry1_nlb_ydb)。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|method|String|是|指定操作类型，取值：update。|
|state|String|是|设备发送给shadow的具体状态。其中，desired部分是期望的影子状态。|
|version|String|是|设备影子的版本，必须大于当前影子版本。|

## 返回参数 {#section_agx_xb5_j2b .section}

|参数|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|

## 示例 {#section_kxf_jmb_ydb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?&Action=UpdateDeviceShadow
&ProductKey=al*********
&DeviceName=device1
&ShadowMessage=[{"method":"update","state":{"desired":{"color":"green"},"reported":"\"},"version":1}]
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
          "Success":true,
      }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
      <UpdateDeviceShadowResponse>
          <RequestId>BB71E443-4447-4024-A000-EDE09922891E</RequestId>
          <Success>true</Success>
      </UpdateDeviceShadowResponse>
    ```


