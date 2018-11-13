# BatchCheckDeviceNames {#reference_xct_2rz_wdb .reference}

调用该接口在指定产品下批量自定义设备名称。IoT平台将检查名称的合法性。

## 限制说明 {#section_osp_szm_xdb .section}

该接口需要和BatchRegisterDeviceWithApplyId接口结合使用，实现在一个产品下批量注册（即新建）多个设备，并且为每个设备单独命名。单次调用，最多能传入1,000 个设备名称。

首先，您必须先调用本接口，传入要批量注册的设备名称。IoT平台检查您提交的设备名称符合要求后，为您返回申请批次ID（ApplyId）。然后，您可以通过使用ApplyId调用BatchRegisterDeviceWithApplyId接口，批量注册设备。如果设备名称列表中包含不合法的设备名称，批量申请设备失败。可调用QueryBatchRegisterDeviceStatus接口检查设备名称是否合法，其返回结果中会提示不合法的设备名称列表。

如果您想在一个产品下批量注册多个设备，且不指定设备名称，设备名称由系统随机生成，您可以调用BatchRegisterDevice接口。

如果您想在一个产品下单独注册一个设备，您可以调用RegisterDevice接口。

## 请求参数 {#section_hkz_p1n_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchCheckDeviceNames。|
|ProductKey|String|是|要批量注册的设备所隶属的产品Key。|
|DeviceName|List<String\>|是| 要批量注册的设备的名称列表。每个设备名称应包含4-32个字符，可以包含英文字母、数字和特殊字符（连字符、下划线、@符号、点号、和英文冒号）。

 **说明：** 单次调用，最多能传入1,000个设备名称。

 |
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_xy2_qbn_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|ApplyId|Long|调用成功时，系统返回的申请批次ID。使用该ApplyId，调用BatchRegisterDeviceWithApplyId接口来批量创建设备。|

## 示例 {#section_fts_bcn_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchCheckDeviceNames
&productKey=al**********
&DeviceName.1=device1
&DeviceName.3=device2
&DeviceName.2=device3
&DeviceName.4=device4
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "ApplyId": 4399
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <BatchCheckDeviceNamesResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <ApplyId>4401</ApplyId>
        </Data>
    </BatchCheckDeviceNamesResponse>
    ```


