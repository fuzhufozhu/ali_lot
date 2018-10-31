# GetThingTopo {#reference_bhf_srz_wdb .reference}

调用该接口查询指定设备的拓扑关系。

## 请求参数 {#section_rfl_5mt_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：GetThingTopo。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|PageSize|Integer|是|返回结果中每页显示的记录数量。数量限制：每页最多可显示50条记录。|
|PageNo|Integer|是|从返回结果中的第几页开始显示。|
|公共请求参数|-|是|请参见[公共参数](intl.zh-CN/云端开发指南/云端API参考/公共参数.md#)。|

## 返回参数 {#section_xgj_gnt_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Code|String|调用失败时，返回的错误码。错误码详情，请参见[错误码](intl.zh-CN/云端开发指南/云端API参考/错误码.md#)。|
|Data|Data|调用成功时，返回的设备信息集合。详情参见[DeviceDataInfo](#table_av3_mnt_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|PageCount|Integer|总页数。|
|PageSize|Integer|每页显示的记录数。|
|List|List|设备信息集合。每个元素的结构参见[DeviceInfo](#table_s3w_5nt_xdb)。|
|CurrentPage|Integer|当前页面号。|
|Total|Integer|总记录数。|

|名称|类型|描述|
|:-|:-|:-|
|IotId|String|设备的ID。|
|ProductKey|String|设备所隶属的产品Key。|
|DeviceName|String|设备名称。|
|DeviceSecret|String|设备密钥。|

## 示例 {#section_zn5_g4t_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=GetThingTopo
&ProductKey=al*********
&DeviceName=device1
&PageSize=10
&PageNo=1
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
       "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true
        "Data": {
            "PageCount": 1,
            "PageSize": 10,
            "List": {
                "deviceInfo": [
                    {
                        "DeviceName": "device1",
                        "ProductKey": "al*********",
    		    "DeviceSecret":"abc******"
                        "IotId": "SR8FiTu1R9tlUR2V1bmi0010*****"
                    }
                ]
            },
            "CurrentPage": 1,
            "Total": 1
        },
        
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='UTF-8'?>
    <GetThingTopoResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <PageCount>1</PageCount>
            <PageSize>10</PageSize>
            <List>
                <DeviceInfo>
                    <DeviceName>device1</DeviceName>
                    <ProductKey>al*********</ProductKey>
                    <DeviceSecret>abc******</DeviceSecret>
                    <IotId>SR8FiTu1R9tlUR2V1bmi0010*****</IotId>
                </DeviceInfo>
            </List>
            <CurrentPage>1</CurrentPage>
            <Total>1</Total>
        </Data>
    </GetThingTopoResponse>
    ```


