# Ruff开发板接入物联网平台 {#concept_hhw_xsv_vgb .concept}

您可以使用Ruff开发板开发物联网应用，并接入阿里云物联网平台，便可通过阿里云物联网平台远程控制您的Ruff应用服务器。本文档以开发一个空气质量监控应用为例，介绍将Ruff应用服务器接入物联网平台的配置过程。

## 背景信息 {#section_v5f_dtv_vgb .section}

阿里云物联网平台支持多种开发板服务器接入，以实现对应用服务器的远程控制。应用服务器将设备数据传入物联网平台后，可通过规则引擎的数据流传功能将设备数据流转至其他支持的阿里云服务中进行存储、分析、计算等处理。如下图所示。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/127995/155075256339148_zh-CN.png)

本文档中，只介绍开发Ruff应用服务器，并连接物联网平台；在物联网平台上，设置数据流传规则将服务器上报的设备数据流转至表格存储。

上图中，其他操作流程请参见：

-   [Android Things接入物联网平台](cn.zh-CN/最佳实践/Android Things接入物联网平台.md#)
-   [数据转发至函数计算](../../../../../cn.zh-CN/用户指南/规则引擎/数据流转使用示例/转发数据到函数计算.md#)
-   [上报数据到钉钉群机器人](cn.zh-CN/最佳实践/温湿度计上报数据到钉钉群机器人实践.md#)
-   [DataV数据可视化](https://help.aliyun.com/document_detail/30360.html)

## 前提条件 {#section_yhf_2tv_vgb .section}

本文档示例操作的前提条件：

-   开通[阿里云物联网平台服务](https://www.aliyun.com/product/iot-deviceconnect)。
-   开通[阿里云表格存储服务](https://www.aliyun.com/product/ots)。
-   准备Ruff开发板。

## 操作流程 {#section_ibz_2tv_vgb .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com)，创建产品和设备。
    1.  创建产品。如需帮助，请参见[创建产品](../../../../../cn.zh-CN/用户指南/产品与设备/创建产品(高级版).md#)。
    2.  在产品详情页的Topic类列表页，单击**定义Topic类**，然后自定义一个空气质量监测设备用于发布数据的Topic类。

        示例Topic类信息如下：

        |Topic|操作权限|说明|
        |:----|:---|:-|
        |/$\{productKey\}/$\{deviceName\}/pm25data|发布|用于设备上报数据。上报数据payload如：`{"pm25":23,"pm10":63}`。

|

    3.  单击左侧导航栏选择**设备**，进入设备管理页，并在产品下创建设备。
2.  在[表格存储控制台](https://ots.console.aliyun.com/index)，创建实例和表格。

    1.  在实例列表页，单击**创建实例**，创建一个实例。
    2.  单击新建实例对应的**管理**按钮。
    3.  在实例详情页，单击右上角**创建数据表**按钮，并创建一个数据表，并且添加两个表主键：deviceId （对应值为设备名称）和time（对应值为设备数据上报时间）。
    表格存储使用指南，请参见[表格存储文档](https://help.aliyun.com/document_detail/55211.html)。

3.  在物联网平台控制台规则引擎中，创建一个将设备数据流转到表格存储的规则。

    1.  在单击规则引擎的数据流转页，单击**创建规则**，并创建一个规则。
    2.  在该规则的数据流转详情页，单击**编写SQL**，然后编写用于筛选处理空气质量检测设备上报数据的SQL。

        本示例中SQL如下：

        ```
        SELECT deviceName() as deviceName , timestamp('yyyy-MM-dd HH:mm:ss') as time, pm25, pm10 FROM "/a1jhoQasrGY/+/pm25data"
        ```

        其中，函数 `deviceName()`代表设备名称，`timestamp('yyyy-MM-dd HH:mm:ss')`代表数据上报的时间；数据来源为Topic `/a1jhoQasrGY/+/pm25data`，即该产品下所有设备通过这个Topic上报的消息。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/127995/155075256339155_zh-CN.png)

    3.  单击转发数据对应的**添加操作**，并设置将设备数据转发到表格存储数据表中。

        如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/127995/155075256339156_zh-CN.png)

    4.  返回规则列表页，单击该规则对应的**启动**按钮启动规则。
    数据流转规则设置并启动后，设备上报的对应数据将会被流转到设置的表格存储数据表中。

4.  开发设备端SDK。

    以下示例是在Linux系统上操作。

    1.  输入 mkdir apsarasCampusAir创建一个名为apsarasCampusAir的文件夹。
    2.  输入cd apsarasCampusAir进入该文件夹。
    3.  输入rap init创建工程。
    4.  输入rap device add air \(SDS011\)添加硬件驱动。
    5.  在package.json中增加物联网平台SDK包 `aliyun-iot-device-mqtt`。

        ```
        {
            "name": "apsarascampusair",
            "version": "0.1.0",
            "description": "",
            "author": "",
            "main": "src/index.js",
            "ruff": {
                "dependencies": {
                    "aliyun-iot-device-mqtt": "^0.0.5",
                    "sds011": "^1.1.0"
                },
                "version": 1
            }
        }
        ```

    6.  使用`$npm install`命令，安装阿里云物联网平台设备端SDK。物联网平台SDK下载地址，请参见[下载设备端SDK](../../../../../cn.zh-CN/设备端开发指南/下载设备端SDK.md#)。
    7.  修改`index.js`主程序。

        ```
        // 引入aliyun-iot-sdk
        var MQTT = require('aliyun-iot-device-mqtt');
        
        // 设备信息
        var options = {
            productKey: "", //替换为您的产品的ProductKey
            deviceName: "", //替换为您的设备名称DeviceName
            deviceSecret: "", //替换为您的设备DeviceSecret
            regionId: "", //替换为您的产品设备所在地域
        };
        
        var pm25Data = 0;
        var pm10Data = 0;
        
        // 发布/订阅 topic
        var pubTopic = "/" + options.productKey + "/" + options.deviceName + "/pm25data";
        
        // 建立连接
        var client = MQTT.createAliyunIotMqttClient(options);
        
        $.ready(function(error) {
            if (error) {
                console.log(error);
                return;
            }
            //10s上报一次
            setInterval(publishData, 15 * 1000);
        
            //空气质量
            $('#air').on('aqi', function(error, pm25, pm10) {
                if (error) {
                    console.log(error);
                    return;
                }
                pm25Data = pm25;
                pm10Data = pm10;
            });
        });
        
        
        //上报温湿度
        function publishData() {
            var data = {
                "pm25": pm25Data,
                "pm10": pm10Data
            };
        
            console.log(JSON.stringify(data))
        
            client.publish(pubTopic, JSON.stringify(data));
        
        }
        ```

        完整的SDK文件目录结构：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/127995/155075256439157_zh-CN.png)

5.  使用`$rap deploy -s`命令，将SDK发布到Ruff开发板。

Ruff开发板连网后，即可向物联网平台发送数据。

