# QueryPageByApplyId {#reference_js4_3rz_wdb .reference}

调用该接口查询批量注册的设备信息。

## 请求参数 {#section_nxk_cmr_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryPageByApplyId。|
|ApplyId|Long|是|要查询的申请批次ID。申请批次ID可在BatchRegisterDeviceWithApplyId和BatchRegisterDevice接口返回结果中查看。|
|PageSize|Integer|是|指定返回结果中每页显示的记录数量。数量限制：每页最多可显示50条。|
|CurrentPage|Integer|是|指定从返回结果中的第几页开始显示。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_t3z_qmr_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页面号。|
|Total|Integer|总记录数。|
|ApplyDeviceList|[ApplyDeviceInfo](#table_grm_1nr_xdb)|调用成功时生成的已注册的设备列表。详情参见下表。|

|名称|类型|描述|
|:-|:-|:-|
|DeviceId|String| 设备ID（旧版）。

 **说明：** 该参数是旧版本遗留参数，已无实际作用，不能用来标识设备。如果您想要查找具体设备的IotId（即有效的设备ID，设备的唯一标识），您可以调用QueryDeviceDetail接口，传入ProductKey和DeviceName，即可在返回结果中查看有效的IotId。其中设备名称（DeviceName）可在本接口的返回结果中查看。

 |
|DeviceName|String|设备名称。|
|DeviceSecret|String|设备密钥。**说明：** 请妥善保管该信息。

|

## 示例 {#section_fjv_r4r_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryPageByApplyId
&ApplyId=4416
&PageSize=10
&CurrentPage=1
&公共请求参数
```

**返回示例**

```
{
    "RequestId":"BB71E443-4447-4024-A000-EDE09922891E",
    "Success":true,
    Page:1,
    PageSize:10,
    PageCount:1,
    Total:4,
    ApplyDeviceList:{
        ApplyDeviceInfo:[
            {DeviceId:abcdef1234*************,DeviceName:device1,DeviceSecret:abcdefsddd},
            {DeviceId:edcba4321**************,DeviceName:device2,DeviceSecret:daffdfdeaa}
            ]
    }
}
```

