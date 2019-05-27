# LoRaWAN设备数据解析 {#concept_185360 .concept}

LoRaWAN设备与物联网平台的通信数据格式为透传/自定义，因此需要使用数据解析脚本，解析上下行数据。本文以LoRaWAN温湿度传感器为例，介绍LoRaWAN设备数据解析脚本的编辑和调试方法。

## 步骤一：编辑脚本 {#section_bq9_ruk_eey .section}

1.  在[物联网平台控制台](https://iot.console.aliyun.com/lk/summary)，创建连网方式为LoRaWAN的产品。
2.  为该产品定义功能。功能定义具体方法，请参见[新增物模型](cn.zh-CN/用户指南/产品与设备/物模型/新增物模型.md#)。

    本示例中，定义了以下属性、事件和服务。

    |标识符|数据类型|取值范围|读写类型|
    |---|----|----|----|
    |Temperature|int32|-40~55|读写|
    |Humidity|int32|1~100|读写|

    |标识符|调用方式|输入参数|
    |---|----|----|
    |SetTempHumiThreshold|异步|四个输入参数，数据类型均为int32：     -   温度过高告警阈值（标识符：MaxTemp）
    -   温度过低告警阈值（标识符：MinTemp）
    -   湿度过高告警阈值（标识符：MaxHumi）
    -   湿度过低告警阈值（标识符：MinHumi）
 |

    |标识符|事件类型|输入参数|
    |---|----|----|
    |TempError|告警|温度Temperature|
    |HumiError|告警|湿度Humidity|

3.  编写脚本。

    在产品的产品详情页**数据解析**页签下编写脚本。

    脚本中需定义以下两个方法：

    -   将Alink JSON格式数据转为设备自定义格式数据：protocolToRawData。
    -   将设备自定义数据格式转Alink JSON格式数据：rawDataToProtocol。
    脚本中，解析下行数据的函数`protocolToRawData`中，需设定输出结果的起始三个字节，用于指定下行端口号和下行消息类型。否则，系统会丢掉下行帧。 设备实际接收的数据中不会包含这三个字节。

    |LoRa Downlink|Size \(bytes\)|说明|
    |-------------|--------------|--|
    |DFlag|1|固定为0x5D。|
    |FPort|1|下行端口号。|
    |DHDR|1|可选：     -   0：表示 “Unconfirmed Data Down”数据帧。
    -   1：表示 “Confirmed Data Down”数据帧。
 |

    例如，`0x5D 0x0A 0x00`表示下行帧端口号为10，数据帧为“Unconfirmed Data Down”。

    完整的脚本示例Demo，请参见本文附录章节。


## 步骤二： 在线测试脚本 {#section_b8k_hle_bi0 .section}

在数据解析编辑器中，使用模拟数据测试脚本。

-   设备上报数据模拟解析

    在**模拟输入**框中，输入模拟数据，如000102，选择模拟类型为**设备上报数据**，单击**运行**。右侧运行结果中将显示数据解析是否成功。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/248945/155896044247845_zh-CN.png)

-   设备接收数据模拟解析

    输入JSON格式的云端下行模拟数据，选择模拟类型为**设备接收数据**，单击**运行**。

    ``` {#codeblock_vhn_8a4_cd2}
    {
        "method": "thing.service.SetTempHumiThreshold",
        "id": "12345",
        "version": "1.1",
        "params": {
            "MaxTemp": 50,
            "MinTemp": 8,
            "MaxHumi": 90,
            "MinHumi": 10
        }
    }
    ```

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/248945/155896044247846_zh-CN.png)


## 步骤三：提交脚本 {#section_ntl_8fm_mzi .section}

确认脚本可以正确解析数据后，单击**提交**，将该脚本提交到物联网平台系统，以供数据上下行时，物联网平台调用该脚本解析数据。

**说明：** 仅提交后的脚本才能被物联网平台调用；草稿状态的脚本不能被调用。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/248945/155896044247848_zh-CN.png)

## 步骤四：使用真实设备调试 {#section_ufx_8d8_9nv .section}

脚本提交后，正式使用之前，请使用真实设备进行测试。LoRaWAN节点设备如何发送和接收数据，请参考模组厂商的相关手册。

-   测试LoRaWAN设备上报温湿度属性。

    1.  使用设备端发送数据，如000102。

    2.  在该设备的设备详情页**运行状态**页签下，查看设备上报的属性数据。

-   测试LoRaWAN设备上报事件。

    1.  使用设备端发送事件数据，如，温度告警数据0102，或湿度告警数据0202。

    2.  在该设备的设备详情页**事件管理**页签下，查看设备上报的事件数据。

-   测试调用LoRaWAN设备服务。

    1.  在物联网平台控制台，选择**监控运维** \> **在线调试**。

    2.  选择要调试的产品和设备，并选择**调试真实设备**，功能选择为**温度湿度阈值（SetTempHumiThreshold）**，输入以下数据后，单击**发送指令**。

        ``` {#codeblock_hrj_hf7_kiu}
        {
                "MaxTemp": 50,
                "MinTemp": 8,
                "MaxHumi": 90,
                "MinHumi": 10
            }
        ```

    3.  检查设备端是否接收到服务调用命令。

    4.  在该设备的设备详情页**服务调用**页签下，查看设备服务调用数据。


查看设备属性、事件和服务调用数据的路径如下图：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/248945/155896044247847_zh-CN.png)

## 附录：示例脚本 {#section_k9w_9id_mrb .section}

根据以上产品和其功能定义，编辑的示例脚本如下：

``` {#codeblock_bfc_iy2_out}
var ALINK_ID = "12345";
var ALINK_VERSION = "1.1";
var ALINK_PROP_POST_METHOD     = 'thing.event.property.post';
var ALINK_EVENT_TEMPERR_METHOD = 'thing.event.TempError.post';
var ALINK_EVENT_HUMIERR_METHOD = 'thing.event.HumiError.post';
var ALINK_PROP_SET_METHOD      = 'thing.service.property.set';
var ALINK_SERVICE_THSET_METHOD = 'thing.service.SetTempHumiThreshold';
/*
 * 示例数据：
 *  传入参数 ->
 *      000102 // 共3个字节
 *  输出结果 ->
 *      {"method":"thing.event.property.post", "id":"12345", "params":{"Temperature":1,"Humidity":2}, "version":"1.1"}
 *  传入参数 ->
 *      0102 // 共2个字节
 *  输出结果 ->
 *      {"method":"thing.event.TempError.post","id":"12345","params":{"Temperature":2},"version":"1.1"}
 *  传入参数 ->
 *      0202 // 共2个字节
 *  输出结果 ->
 *     {"method":"thing.event.HumiError.post","id":"12345","params":{"Humidity":2},"version":"1.1"}
 */
function rawDataToProtocol(bytes)
{
    var uint8Array = new Uint8Array(bytes.length);
    for (var i = 0; i < bytes.length; i++)
    {
        uint8Array[i] = bytes[i] & 0xff;
    }
    var params = {};
    var jsonMap = {};
    var dataView = new DataView(uint8Array.buffer, 0);
    var cmd = uint8Array[0]; // command
    if (cmd === 0x00)
    {
        params['Temperature']  = dataView.getInt8(1);
        params['Humidity']     = dataView.getInt8(2);
        jsonMap['method']  = ALINK_PROP_POST_METHOD;
    }
    else if (cmd == 0x01)
    {
        params['Temperature']  = dataView.getInt8(1);
        jsonMap['method']  = ALINK_EVENT_TEMPERR_METHOD;
    }
    else if (cmd == 0x02)
    {
        params['Humidity']  = dataView.getInt8(1);
        jsonMap['method']  = ALINK_EVENT_HUMIERR_METHOD;
    }
    else
    {
        return null;
    }
    jsonMap['version'] = ALINK_VERSION;
    jsonMap['id']      = ALINK_ID;
    jsonMap['params']  = params;
    return jsonMap;
}
/*
 * 示例数据：
 *  传入参数 ->
 *      {"method":"thing.service.SetTempHumiThreshold", "id":"12345", "version":"1.1", "params":{"MaxTemp":50, "MinTemp":8, "MaxHumi":90, "MinHumi":10}}
 *  输出结果 ->
 *      0x5d0a000332085a0a
 */
function protocolToRawData(json)
{
    var id  = json['id'];
    var method  = json['method'];
    var version = json['version'];
    var payloadArray = [];
    // 追加下行帧头部
    payloadArray = payloadArray.concat(0x5d);
    payloadArray = payloadArray.concat(0x0a);
    payloadArray = payloadArray.concat(0x00);
    if (method == ALINK_SERVICE_THSET_METHOD)
    {
        var params  = json['params'];
        var maxtemp = params['MaxTemp'];
        var mintemp = params['MinTemp'];
        var maxhumi = params['MaxHumi'];
        var minhumi = params['MinHumi'];
        payloadArray = payloadArray.concat(0x03);
        if (maxtemp !== null)
        {
            payloadArray = payloadArray.concat(maxtemp);
        }
        if (mintemp !== null)
        {
            payloadArray = payloadArray.concat(mintemp);
        }
        if (maxhumi !== null)
        {
            payloadArray = payloadArray.concat(maxhumi);
        }
        if (minhumi !== null)
        {
            payloadArray = payloadArray.concat(minhumi);
        }
    }
    return payloadArray;
}
// 以下是部分辅助函数
function buffer_uint8(value)
{
    var uint8Array = new Uint8Array(1);
    var dv = new DataView(uint8Array.buffer, 0);
    dv.setUint8(0, value);
    return [].slice.call(uint8Array);
}
function buffer_int16(value)
{
    var uint8Array = new Uint8Array(2);
    var dv = new DataView(uint8Array.buffer, 0);
    dv.setInt16(0, value);
    return [].slice.call(uint8Array);
}
function buffer_int32(value)
{
    var uint8Array = new Uint8Array(4);
    var dv = new DataView(uint8Array.buffer, 0);
    dv.setInt32(0, value);
    return [].slice.call(uint8Array);
}
function buffer_float32(value)
{
    var uint8Array = new Uint8Array(4);
    var dv = new DataView(uint8Array.buffer, 0);
    dv.setFloat32(0, value);
    return [].slice.call(uint8Array);
}
```

## 相关文档 { .section}

-   了解数据解析流程和脚本格式等基本信息，请参见[什么是数据解析](cn.zh-CN/用户指南/产品与设备/数据解析/什么是数据解析.md#)。
-   关于脚本解析问题排查，请参见[问题排查](cn.zh-CN/用户指南/产品与设备/数据解析/问题排查.md#)。

