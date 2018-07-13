# GetDeviceStatus {#reference_zdy_wqz_wdb .reference}

调用该接口查看指定设备的运行状态。

## 请求参数 {#section_ujp_1gm_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetDeviceStatus。|
|IotId|String|否| 要查看运行状态的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey&DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey&DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查看运行状态的设备隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 指定要查看运行状态的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |

## 返回参数 {#section_dtb_mgm_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|DeviceStatusInfo|调用成功时，返回设备状态信息。详情参见[DeviceStatusInfo](#table_glr_1hm_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|Status|String| 设备状态。取值：

 ONLINE：设备在线。

 OFFLINE：设备离线。

 UNACTIVE：设备未激活。

 DISABLE：设备已禁用。

 |

## 示例 {#section_e2g_mhm_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetDeviceStatus
 &IotId=MpEKNuEUJzIORNANAWJX0010929900*****
 &公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true
      "Data": {
        "Status": "UNACTIVE"
      } 
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <GetDeviceStatusResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <Status>UNACTIVE</Status>
        </Data>
    </GetDeviceStatusResponse>
    ```


