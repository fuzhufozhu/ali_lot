# BatchRegisterDeviceWithApplyId {#reference_iwr_frz_wdb .reference}

调用该接口根据申请批次ID（ApplyId）批量注册设备。

## 限制说明 {#section_g31_hdn_xdb .section}

该接口需要和BatchCheckDeviceNames接口结合使用，实现在一个产品下批量注册（即新建）多个设备，并且为每个设备单独命名。

首先，您必须调用BatchCheckDeviceNames接口，传入要批量注册的设备的名称。IoT平台检查您提交的设备名称符合要求后，为您返回申请批次ID（ApplyId）。然后，您可以通过使用ApplyId调用本接口，批量注册设备。

如果您想在一个产品下批量注册多个设备，且不指定设备名称，设备名称由系统随机生成，您可以调用BatchRegisterDevice接口。

如果您想在一个产品下单独注册一个设备，您可以调用RegisterDevice接口。

## 请求参数 {#section_ojj_pdn_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchRegisterDeviceWithApplyId。|
|ProductKey|String|是|要批量注册的设备所隶属的产品Key。|
|ApplyId|Long|是|要批量注册的设备的申请批次ID。申请批次ID由调用BatchCheckDeviceNames接口返回。|

## 返回参数 {#section_iv1_ydn_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|Data|调用成功时，返回的数据。详情见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|ApplyId|Long|传入的申请批次ID。|

## 示例 {#section_emq_f2n_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchRegisterDeviceWithApplyId
&ProductKey=al*********
&ApplyId=4416
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "ApplyId": 4416
      }
    }
    
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <BatchRegisterDeviceWithApplyId>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <ApplyId>4416</ApplyId>
        </Data>
    </BatchRegisterDeviceWithApplyId>
    ```


