# 将数据转发至时序时空数据库\(TSDB\) {#concept_xzj_pc1_hfb .concept}

本章以采集楼层中传感器数据为例，介绍将数据转发至时序时空数据库（TSDB）的数据流转规则设置。

## 场景 {#section_ocx_dg1_kfb .section}

在No-1大厦的两个楼层中（比如，F1和F2层），每层分布2个传感器来记录该楼层的温度、湿度、PM2.5、甲醛含量等环境信息。

传感器每5秒采集一次环境数据并上报至物联网平台，物联网平台通过设置好的数据流转规则将环境数据转发到TSDB。您可以利用TSDB的空间聚合和降采样能力轻松实现数据统计与分析。

## 前提条件 {#section_i4f_ml5_mfb .section}

在控制台创建传感器产品和设备，并将设备连接到物联网平台。具体请参考[快速入门](../../../../cn.zh-CN/快速入门/创建产品与设备.md#)。

## 数据上报 {#section_bd2_2g1_kfb .section}

-   数据上报频率：1次/5s。
-   数据上报Topic：`/${productKey}/${deviceName}/user/data`
-   payload格式：

    ``` {#codeblock_ayx_xcc_agg}
    {"temperature":25,"humidity":24,"pm25":11,"hcho":0.02}
    ```


## 配置规则 {#section_cw2_2g1_kfb .section}

1.  登录[物联网平台控制台](https://iot.console.aliyun.com/)。
2.  选择**规则引擎**，单击**创建规则**，创建JSON数据格式规则。
3.  请参考[设置数据流转规则](../../../../cn.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)，编写处理数据的SQL。本示例中的SQL如下：

    ``` {#codeblock_na5_cod_q9t}
    SELECT deviceName() as deviceName, timestamp() as time, attribute('floor') as floor, attribute('building') as building, temperature, humidity, pm25, hcho FROM "/${productKey}/+/user/data"
    ```

4.  添加转发数据操作。

    设置将数据转发到一个TSDB的VPC实例中。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21860/156291757313340_zh-CN.png)


## 时序数据使用 {#section_n4f_2g1_kfb .section}

1.  登录[时序时空数据库控制台](https://tsdb.console.aliyun.com)。
2.  在实例列表中找到存储数据的VPC实例，并单击右侧**管理**。
3.  参考[时序洞察](https://help.aliyun.com/document_detail/89056.html)中的查询数据操作步骤，查询IoT发送到实例中的数据。
    -   按楼层聚合对比数据，根据如下表格设置查询参数：

        |参数|取值|
        |--|--|
        |空间聚合函数|选择avg|
        |标签|building=No-1|
        |分组|选择floor|

        数据查询结果如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21860/156291757313341_zh-CN.png)

    -   按大厦聚合avg，降采样1分钟查询数据，根据如下表格设置查询参数：

        |参数|取值|
        |--|--|
        |空间聚合函数|选择avg|
        |标签|此处添加如下两个标签：         -   building=No-1
        -   floor=f1/f2
 |
        |降采样|打开开关|
        |降采样聚合函数|选择max|
        |采样时间间隔|选择1分钟|

        数据查询结果如下图所示：

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21860/156291757413342_zh-CN.png)


