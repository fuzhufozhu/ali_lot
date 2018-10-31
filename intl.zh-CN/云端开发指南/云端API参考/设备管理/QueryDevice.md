# QueryDevice {#reference_vhm_npz_wdb .reference}

用该接口查询指定产品下的所有设备列表。

## 请求参数 {#section_tnd_w1m_xdb .section}

|名称|类型|是否必须需|描述|
|:-|:-|:----|:-|
|Action|String|是|要执行的操作，取值：QueryDevice。|
|ProductKey|String|是|要查询的设备所隶属的产品Key。|
|PageSize|Integer|否|指定返回结果中每页显示的记录数量，最大值是50。默认值是10。|
|CurrentPage|Integer|否|指定从返回结果中的第几页开始显示。默认值是 1。|
|公共请求参数|-|是|公共请求参数，请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#) 。|

## 返回参数 {#section_m3x_3bm_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|Page|Integer|当前页面号。|
|Total|Integer|总记录数。|
|Data|Data|调用成功时，返回设备信息列表。详情参见下表[DeviceInfo](#table_qxz_rbm_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|DeviceId|String| 设备ID（旧版参数）。

 **说明：** 该参数是旧版本遗留参数，已无实际作用，不能用来标识设备。如果您想要查找具体设备的IotId（即有效的设备ID，设备的唯一标识），您可以调用QueryDeviceDetail接口，传入ProductKey和DeviceName组合。

 |
|DeviceName|String|设备名称。|
|DeviceSecret|String|设备密钥。|
|GmtCreate|String|设备创建时间。|
|GmtModified|String|设备信息修改时间。|

## 示例 {#section_a12_3cm_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDevice
&ProductKey=al*********
&PageSize=10
&CurrentPage=1
&公共请求参数数
```

**返回示例**

-   JSON格式

    ```
    {
          PageCount:1, 
          Data:{
              DeviceInfo:[
                  {DeviceId:SR8FiTu1R9tlUR2V1bmi0010*****, DeviceName:device1, DeviceSecret:CPwUjMUgzdvaZv56TMy6773V3v3****, GmtCreate:Thu, 17-Nov-2016 02:08:12 GMT, GmtModified:Thu, 17-Nov-2016 02:08:12 GMT}
              ]
          }, 
          PageSize:10, 
          Page:1, 
          Total:9
          RequestId:06DC77A0-4622-42DB-9EE0-26A6E1FA08D3, 
          Success:true, 
      }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
     <QueryDeviceResponse>
          <RequestId>9D25B7F9-DF08-4F77-B1E9-008F78522B37</RequestId>
          <Success>true</Success>
          <Page>1</Page>
          <PageSize>10</PageSize>
          <PageCount>1</PageCount>
          <Total>6</Total>
          <Data>
              <DeviceInfo>
                  <DeviceId>SR8FiTu1R9tlUR2V1bmi0010*****</DeviceId>
                  <DeviceName>device1</DeviceName>
                  <DeviceSecret>CPwUjMUgzdvaZv56TMy6773V3v3****</DeviceSecret>
                  <GmtCreate>Thu, 10-Nov-2016 05:35:56 GMT</GmtCreate>
                  <GmtModified>Thu, 10-Nov-2016 05:35:56 GMT</GmtModified>
              </DeviceInfo>
              ...
          </Data>
      </QueryDeviceResponse>
    ```


