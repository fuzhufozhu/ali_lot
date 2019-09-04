# NotifyAddThingTopo {#reference_fhb_trz_wdb .reference}

调用该接口通知网关设备增加拓扑关系。

## 使用说明 {#section_avc_pe4_1q2 .section}

-   返回的成功结果只表示添加拓扑关系的指令成功下发给网关，但并不表示网关成功添加拓扑关系。
-   开发网关设备端时，需订阅通知添加拓扑关系消息的Topic。具体Topic和消息格式，请参见[通知网关添加设备拓扑关系](../../../../intl.zh-CN/设备端开发指南/基于Alink协议开发/管理拓扑关系.md#section_cn4_zzw_y2b)。

## 请求参数 {#section_zl1_wqt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：NotifyAddThingTopo。|
|GwIotId|String|否| 要通知的网关设备ID，即网关类型设备的IotId。

 **说明：** 如果传入该参数，则无需传入GwProductKey和GwDeviceName。GwIotId作为设备唯一标识符，与GwProductKey和GwDeviceName组合是一一对应的关系。如果您同时传入GwIotId和GwProductKey与GwDeviceName组合，则以GwIotId为准。

 |
|GwProductKey|String|否| 要通知的网关设备所隶属的产品Key，即网关类型产品的ProductKey。

 **说明：** 如果传入该参数，需同时传入GwDeviceName。

 |
|GwDeviceName|String|否| 要通知的网关设备的名称，即网关类型设备的DeviceName。

 **说明：** 如果传入该参数，需同时传入GwProductKey。

 |
|DeviceListStr|List|是|要挂载在目标网关设备上的子设备数组，为JSON字符串形式。具体结构参见下表DeviceList。|
|IotInstanceId|String|否|公共实例不传此参数；仅独享实例需传入实例ID。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|ProductKey|String|否| 子设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 子设备名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|IotId|String|否| 子设备ID，物联网平台颁发给设备的唯一标识。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey & DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |

## 返回参数 {#section_qrj_cst_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情请见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|MessageId|String|云端向网关设备下发增加拓扑关系的消息ID。|

## 示例 {#section_zl2_jst_xdb .section}

请求示例

``` {#codeblock_v4b_ft5_1a4}
https://iot.cn-shanghai.aliyuncs.com/?Action=NotifyAddThingTopo
&GwProductKey=aldnfald7a
&GwDeviceName=gateway
&DeviceListStr=[{"productKey":"alabcabcab","deviceName":"device1"},{"IotId":"edAjkIeBSsdfadjjllja***"}]
&公共请求参数
```

返回示例

-   JSON格式

    ``` {#codeblock_zr5_u3h_rux}
    {
      "RequestId":"419A3FC1-B517-4958-9414-5546765FA51F",
      "Success": true,
      "Data"：{
         "MessageId": "abcabc123"
      }
    }
    ```

-   XML格式

    ``` {#codeblock_373_trq_6ir}
    <?xml version='1.0' encoding='UTF-8'?>
    <NotifyAddThingTopoResponse>
        <RequestId>419A3FC1-B517-4958-9414-5546765FA51F</RequestId>
        <Success>true</Success>
        <Data>
        <MessageId>abcabc123</MessageId>
        </Data>
    </NotifyAddThingTopoResponse>
    ```


