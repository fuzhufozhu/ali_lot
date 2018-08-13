# QueryDevicePropertyData {#reference_q2q_krz_wdb .reference}

调用该接口查询指定设备的属性记录。

## 请求参数 {#section_s1v_mss_xdb .section}

|名称|类型|是否必需|描述|
|:-|:-|:---|:-|
|Action|String|是|要执行的操作，取值：QueryDevicePropertyData。|
|IotId|String|否| 要查询的设备ID，设备的唯一标识符。

 **说明：** 如果传入该参数，则无需传入ProductKey和DeviceName。IotId作为设备唯一标识符，与ProductKey和DeviceName组合是一一对应的关系。如果您同时传入IotId和ProductKey与DeviceName组合，则以IotId为准。

 |
|ProductKey|String|否| 指定要查询的设备隶属的产品Key。

 **说明：** 如果传入该参数，需同时传入DeviceName。

 |
|DeviceName|String|否| 指定要查询的设备的名称。

 **说明：** 如果传入该参数，需同时传入ProductKey。

 |
|Identifier|String|是| 要查询的属性标识符。

 高级版设备的属性Identifier，可在控制台中设备所属的高级版产品的功能定义中查看。若设备有多个属性，您可以多次调用该接口进行查询，一次输入一个Identifier。

 |
|StartTime|Long|是|要查询的属性记录的开始时间。取值为毫秒值时间戳。|
|EndTime|Long|是|要查询的属性记录的结束时间。取值为毫秒值时间戳。|
|PageSize|Integer|是|返回结果中每页显示的记录数。数量限制：每页最多可显示50条。|
|Asc|Integer|是| 返回结果中属性记录的排序方式，取值：

 0：倒序。

 1：正序。

 |

## 返回参数 {#section_pyc_l5s_xdb .section}

|名称|类型|描述|
|:-|:-|:-|
|RequestId|String|阿里云为该请求生成的唯一标识符。|
|Success|Boolean|表示是否调用成功。true表示调用成功，false表示调用失败。|
|ErrorMessage|String|调用失败时，返回的出错信息。|
|Data|[Data](#table_sdc_7592_xdb)|调用成功时，返回的设备属性记录。详情参见下表。|

|名称|类型|描述|
|:-|:-|:-|
|List|List<PropertyList\>|属性集合。每个元素代表一个属性。元素的结构参见[PropertyInfo](#table_z2b_7593_xdb)。|
|NextValid|Boolean|表示下一页面是否可用。true表示可用，false表示不可用。|
|NextTime|Long|下一页面中的属性记录的起始时间。|

|名称|类型|描述|
|:-|:-|:-|
|Value|String|属性值。|
|Time|Long|属性修改时间。|

## 示例 {#section_ty1_vvs_xdb .section}

**请求示例**

```
https://iot.cn-shanghai.aliyuncs.com/?Action=QueryDevicePropertyData
&IotId=SR8FiTu1R9tlUR2V1bmi0010*****
&ProductKey=al*********
&PeviceName=device1
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
        "NextValid": true,
        "NextTime": 1516541821599,
        "List": {
          "PropertyInfo": [
            {
              "Value": "32",
              "Time": 1516541894876
            },
            {
              "Value": "2",
              "Time": 1516541885630
            },
            {
              "Value": "95",
              "Time": 1516541875947
            },
            {
              "Value": "14",
              "Time": 1516541830905
            },
            {
              "Value": "78",
              "Time": 1516541821600
            }
          ]
        }
      }
    }
    ```

-   XML格式

    ```
    <?xml version='1.0' encoding='utf-8'?>
    <QueryDevicePropertyDataResponse>
        <RequestId>57b144cf-09fc-4916-a272-a62902d5b207</RequestId>
        <Success>true</Success>
        <Data>
            <NextValid>true</NextValid>
            <NextTime>1516541821599</NextTime>
            <List>
                <PropertyInfo>
                    <Value>32</Value>
                    <Time>1516541894876</Time>
                </PropertyInfo>
                <PropertyInfo>
                    <Value>2</Value>
                    <Time>1516541885630</Time>
                </PropertyInfo>
                <PropertyInfo>
                    <Value>95</Value>
                    <Time>1516541875947</Time>
                </PropertyInfo>
                <PropertyInfo>
                    <Value>14</Value>
                    <Time>1516541830905</Time>
                </PropertyInfo>
                <PropertyInfo>
                    <Value>78</Value>
                    <Time>1516541821600</Time>
                </PropertyInfo>
            </List>
        </Data>
    </QueryDevicePropertyDataResponse>
    ```


