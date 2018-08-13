# QueryDeviceEventData {#reference_tq4_jrz_wdb .reference}

调用该接口查询指定设备的事件记录。

## 请求参数 {#section_srt_qqr_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDeviceEventData。|
|IotId|String|否| 要查询的设备ID。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey&DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 要查询的设备所隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|EventType|String|否| 要查询的事件类型。取值：

 info：信息。

 alert：告警。

 error：故障。

 |
|Identifier|String|是|要查询的事件标识符。高级版设备的事件Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。|
|StartTime|Long|是|要查询的事件记录的开始时间。|
|EndTime|Long|是|要查询的事件记录的结束时间。|
|PageSize|Integer|否|返回结果中每页显示的记录数。数量限制：每页最多可显示50条。|
|Asc|Integer|否| 返回结果中事件记录的排序方式，取值：

 0：倒序。

 1：正序。

 |

## 返回参数 {#section_qds_dyr_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|Data|调用成功时，返回的设备事件记录。详情参见[Data](#table_sdc_kyr_xdb)。|

|名称|类型|描述|
|:-|:-|:-|
|List|List<EventInfo\>|事件集合。每个元素代表一个事件。元素的结构参见[EventInfo](#table_hxp_kzr_xdb)。|
|NextValid|Boolean|表示下一页面是否可用。true表示可用，false表示不可用。|
|NextTime|Long|下一页面中的事件记录的起始时间。|

|名称|类型|描述|
|:-|:-|:-|
|Name|String|事件名称。|
|Time|Long|事件发生时间。|
|OutputData|String|事件的输出参数，map格式的字符串。具体结构参见[OutputData](#table_z2b_tzr_xdb)。|
|EventType|String|事件类型。|
|Identifier|String|事件标识符。|

|名称|类型|描述|
|:-|:-|:-|
|Key|String|参数标识符。|
|Value|Long|参数值。|

## 示例 {#section_c5y_b1s_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDeviceEventData
&IotId=abcdef1234567************
&ProductKey=al**********
&DeviceName=device1
&EventType=info
&Identifier=lightLevel
&StartTime=1516538300303L
&EndTime=1516541900303L
&PageSize=10
&Asc=1
&公共请求参数
```

**返回示例**

-   JSON格式

    ```
    {
      "RequestId":"57b144cf-09fc-4916-a272-a62902d5b207",
      "Success": true，
      "Data": {
        "NextValid": false,
        "NextTime": 1516449221253,
        "List": {
          "EventInfo": [
            {
              "Name": "testEventInfoName",
              "Time": "1516517974638",
              "OutputData": "{\"structArgs\":{\"structchildFLOATf71c20e\":1.23,\"structchildINT6b6b626\":3,\"structchildDATE663436a\":\"1516517966152\",\"structchildDOUBLE08d0f74\":1.23,\"structchildTEXTdc764f9\":\"07b68264b0ba42c18e5f\",\"structchildBOOLd260729\":0,\"structchildENUMbe62590\":1},\"enumArgs\":0,\"boolArgs\":0,\"floatArgs\":2.3,\"dateArgs\":\"1516517966152\",\"intArgs\":1,\"doubleArgs\":2.3,\"textArgs\":\"dV56zbkzjBjw1Ti1dA52\"}",
              "EventType": "info",
              "Identifier": "testEventInfo"
            },
            {
              "Name": "testEventInfoName",
              "Time": "1516449221254",
              "OutputData": "{\"structArgs\":{\"structchildFLOATf71c20e\":1.23,\"structchildINT6b6b626\":3,\"structchildDATE663436a\":\"1516449212507\",\"structchildDOUBLE08d0f74\":1.23,\"structchildTEXTdc764f9\":\"a1f3583dde3944289639\",\"structchildBOOLd260729\":0,\"structchildENUMbe62590\":1},\"enumArgs\":0,\"boolArgs\":0,\"floatArgs\":2.3,\"dateArgs\":\"1516449212507\",\"intArgs\":1,\"doubleArgs\":2.3,\"textArgs\":\"1z4XNBvvA7eZw8XViaJp\"}",
              "EventType": "info",
              "Identifier": "testEventInfo"
            }
          ]
        }
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDeviceEventDataResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <NextValid>false</NextValid>
            <NextTime>1516449221253</NextTime>
            <List>
                <EventInfo>
                    <Name>testEventInfoName</Name>
                    <Time>1516517974638</Time>
                    <OutputData>{"structArgs":{"structchildFLOATf71c20e":1.23,"structchildINT6b6b626":3,"structchildDATE663436a":"1516517966152","structchildDOUBLE08d0f74":1.23,"structchildTEXTdc764f9":"07b68264b0ba42c18e5f","structchildBOOLd260729":0,"structchildENUMbe62590":1},"enumArgs":0,"boolArgs":0,"floatArgs":2.3,"dateArgs":"1516517966152","intArgs":1,"doubleArgs":2.3,"textArgs":"dV56zbkzjBjw1Ti1dA52"}</OutputData>
                    <EventType>info</EventType>
                    <Identifier>testEventInfo</Identifier>
                </EventInfo>
                <EventInfo>
                    <Name>testEventInfoName</Name>
                    <Time>1516449221254</Time>
                    <OutputData>{"structArgs":{"structchildFLOATf71c20e":1.23,"structchildINT6b6b626":3,"structchildDATE663436a":"1516449212507","structchildDOUBLE08d0f74":1.23,"structchildTEXTdc764f9":"a1f3583dde3944289639","structchildBOOLd260729":0,"structchildENUMbe62590":1},"enumArgs":0,"boolArgs":0,"floatArgs":2.3,"dateArgs":"1516449212507","intArgs":1,"doubleArgs":2.3,"textArgs":"1z4XNBvvA7eZw8XViaJp"}</OutputData>
                    <EventType>info</EventType>
                    <Identifier>testEventInfo</Identifier>
                </EventInfo>
            </List>
        </Data>
    </QueryDeviceEventDataResponse>
    ```


