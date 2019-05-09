# CreateLoRaNodesTask {#concept_uk3_mj2_zgb .concept}

调用该接口生成批量注册LoRaWAN设备的任务。

## 使用限制 {#section_wzf_lk2_zgb .section}

单次调用最多可添加500个设备。

## 请求参数 {#section_cnc_qj2_zgb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：CreateLoRaNodesTask。|
|ProductKey|String|是|设备所隶属的产品Key。|
|DeviceInfos|List<DeviceInfo\>|是|LoRaWAN设备信息列表。详情参见下表 DeviceInfo。 **说明：** 单次调用最多可添加500个设备的信息。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](cn.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|DevEui|String|是|LoRaWAN设备的DevEUI，其全球唯一标识。|
|PinCode|String|是|LoRaWAN设备的PIN Code，用于校验DevEUI的合法性。|

## 返回参数 {#section_azr_kzf_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。|
|TaskId|String|调用成功时，返回的任务ID。 **说明：** 请妥善保管该ID。查询设备创建的状态时需使用该ID。

 |

## 示例 {#section_vb1_k42_zgb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=CreateLoRaNodesTask
&DeviceInfo.1.DevEui=d896e0ffffet****
&DeviceInfo.1.PinCode=562959
&DeviceInfo.2.DevEui=d896e0ffffer****
&DeviceInfo.2.PinCode=573091
&ProductKey=a1HfDI4****
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId": "38D5FDA5-19B9-445D-8713-213B743266DE",
      "TaskId": "62146",
      "Success": true
    }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8" ?>
    <CreateLoRaNodesTask>
    	<RequestId>38D5FDA5-19B9-445D-8713-213B743266DE</RequestId>
    	<TaskId>62146</TaskId>
    	<Success>true</Success>
    </CreateLoRaNodesTask>
    ```


