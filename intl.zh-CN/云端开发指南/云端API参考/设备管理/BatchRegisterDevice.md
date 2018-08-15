# BatchRegisterDevice {#reference_dyt_grz_wdb .reference}

调用该接口在指定产品下批量注册多个设备（随机生成设备名）。

## 限制说明 {#section_z1s_w2n_xdb .section}

您可以调用本接口，在一个产品下批量注册多个设备，并且随机生成设备名。单次调用，最多可创建1,000个设备。

如果您想在一个产品下单独注册一个设备，您可以调用RegisterDevice接口。

如果您想在一个产品下批量注册多个设备，并且为每个设备单独命名，您需要分步调用BatchCheckDeviceNames和BatchRegisterDeviceWithApplyId接口。

## 请求参数 {#section_brt_gfn_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchRegisterDevice。|
|ProductKey|String|是|要批量注册的设备所隶属的产品Key。|
|Count|Integer|是|要注册的设备数量，取值不能大于1,000。|

## 返回参数 {#section_hdg_yfn_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|Data|调用成功时，返回的数据。详情见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|ApplyId|Long|调用成功时，系统返回的申请批次ID。|

## 示例 {#section_rwq_vfn_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchRegisterDevice
&ProductKey=al*********
&Count=10
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",  
      "Success": true，
      "Data": {
        "ApplyId": 4415
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <BatchRegisterDeviceResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <ApplyId>4415</ApplyId>
        </Data>
    </BatchRegisterDeviceResponse>
    ```


