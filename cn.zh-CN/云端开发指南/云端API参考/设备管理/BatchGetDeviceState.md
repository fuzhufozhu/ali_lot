# BatchGetDeviceState {#reference_exs_brz_wdb .reference}

调用该接口批量查看同一产品下指定设备的运行状态。

## 限制说明 {#section_nvz_1jm_xdb .section}

该接口用于查看一个产品下多个设备的运行状态，单次最多可查询50个设备。

## 请求参数 {#section_l1s_wjm_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：BatchGetDeviceState。|
|ProductKey|String|是|要查看运行状态的设备所隶属的产品Key。|
|DeviceName|List<String\>|是| 要查看运行状态的设备的名称列表。

 **说明：** 支持单次查询最多50个设备。

 |
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_sts_lsm_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|DeviceStatusList|DeviceStatus|调用成功时，返回设备状态信息列表。详情参见下表[DeviceStatus](#table_thn_ssm_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|DeviceName|String|设备名称。|
|Status|String| 设备状态。取值：

 ONLINE：设备在线。

 OFFLINE：设备离线。

 UNACTIVE：设备未激活。

 DISABLE：设备已禁用。

 |

## 示例 {#section_t5j_zsm_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=BatchGetDeviceState
&productKey=al*********
&DeviceName.1=device1
&DeviceName.2=device2
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
          DeviceStatusList:{
              DeviceStatus:[
                  {Status:UNACTIVE, DeviceName:device1},
                  {Status:UNACTIVE, DeviceName:device2}
              ]
          },
          RequestId:"1A540BD7-176C-42D4-B3C0-A2C549DD00A3",
          Success:true
      }
    ```

-   XML格式

    ```
    <?xml version="1.0" encoding="UTF-8"?> 
      <BatchGetDeviceStateResponse>
          <RequestId>1AB5E6B0-AFCB-47B1-B6D4-C2BD32D63E14</RequestId>
          <Success>true</Success>
          <DeviceStatusList>
              <DeviceStatus>
               <Status>UNACTIVE</Status>
               <DeviceName>device1</DeviceName>
              </DeviceStatus>
              <DeviceStatus>
               <Status>UNACTIVE</Status>
               <DeviceName>device2</DeviceName>
              </DeviceStatus>
          </DeviceStatusList>
      </BatchGetDeviceStateResponse>
    ```


