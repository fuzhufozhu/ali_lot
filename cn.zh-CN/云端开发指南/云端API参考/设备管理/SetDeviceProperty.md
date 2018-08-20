# SetDeviceProperty {#reference_m2h_4rz_wdb .reference}

调用该接口设置设备的属性。因为云端下发属性设置命令和设备收到并执行该命令是异步的，所以调用该接口时，返回的成功结果只表示云端下发属性设置的请求成功，不能保证设备端收到并执行了该请求。需设备端SDK成功响应云端设置设备属性的请求，设备属性才能真正设置成功。

## 请求参数 {#section_jgt_v2t_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：SetDeviceProperty。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Items|Map|是|要设置的属性信息，详情参见[Item](#table_omd_gft_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|key|String| 要设置的属性的标识符（identifier）。高级版设备的事件Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。

 **说明：** 设置的属性必需是读写型。如果您指定了一个只读型的属性，设置将会失败。

 |
|value|Obejct|属性值。取值需跟您定义的属性的数据类型和取值范围保持一致。|

## 返回参数 {#section_znb_2ft_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的数据。详情请参见下表Data。|

|名称|类型|描述|
|:-|:-|:-|
|MessageId|String|云端给设备下发属性设置的消息ID。|

## 示例 {#section_n1g_xft_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=setDeviceProperty
&ProductKey=al*********
&DeviceName=device1
&Items=%7B%22LightAdjustLevel%22%3A1%7D
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true,
      "Data": {
    	  "MessageId":"abcabc123"
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <SetDevicePropertyResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
    	 <MessageId>abcabc123</MessageId>
        </Data>
    </SetDevicePropertyResponse>
    ```


