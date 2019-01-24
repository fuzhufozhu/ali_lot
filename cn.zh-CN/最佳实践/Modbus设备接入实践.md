# Modbus设备接入实践 {#task_nff_2j1_ffb .task}

本文介绍如何将基于Modbus协议的设备接入网关，并与物联网平台交互的方法。

1.  以阿里云账号登录[物联网控制台](http://iot.console.aliyun.com/)。 
2.  参考[创建产品\(高级版\)](../../../../../cn.zh-CN/用户指南/产品与设备/创建产品(高级版).md#)内容，创建网关。 
    1.  创建网关产品。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212514_zh-CN.png)

        其中，设置参数时**节点类型**选择**网关**。

    2.  添加网关设备。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212571_zh-CN.png)

        网关设备添加成功后，请本地保存设备证书（Productkey、DeviceName、DeviceSecret），以备后续使用。

3.  部署网关。 请根据您的设备类型，参考[搭建环境](cn.zh-CN/用户指南/环境搭建/专业版环境搭建/基于Ubuntu 16.04搭建环境.md#)部署网关。部署完成后网关设备显示在线。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212595_zh-CN.png)

4.  创建基于Modbus协议的设备。 
    1.  参考[创建产品\(高级版\)](../../../../../cn.zh-CN/用户指南/产品与设备/创建产品(高级版).md#)，创建环境监测产品。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212609_zh-CN.png)

        其中，

        |参数|描述|
        |--|--|
        |所属分类|选择**智能城市** \> **环境感知** \> **环境监测设备**。|
        |是否接入网关|选择**是**。|
        |接入网关协议|选择**Modbus**。|

    2.  创建完成环境监测产品后，进入产品详情页，开启动态注册。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212610_zh-CN.png)

    3.  参考[新增物模型](../../../../../cn.zh-CN/用户指南/产品与设备/物模型/新增物模型.md#)，在产品详情页，为环境监测产品添加物模型。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364212616_zh-CN.png)

    4.  单击**扩展描述**。 

        在配置物模型属性的过程中，需要把每个属性通过**扩展描述**中的功能映射到Modbus中的寄存器地址，官方Modbus驱动会将所有的属性聚合为Modbus数据请求，驱动收到Modbus数据之后再转换为物模型数据。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364313958_zh-CN.png)

        |名称|描述|
        |--|--|
        |操作类型|指操作Modbus的功能码，具体如下：        -   输入状态，对应的功能码：0x02（读离散输入）
        -   线圈状态，对应的功能码：0x01、0x0F（读、写线圈）
        -   保持寄存器，对应的功能码：0x03、0x10（读、写保持寄存器）
        -   输入寄存器，对应的功能码：0x04（读输入寄存器）
此处选择**保持寄存器**。

 |
        |寄存器地址|指Modbus的寄存器的操作地址|
        |原始数据类型|如采集的温度值的数据类型为浮点型|
        |交换寄存器内高低字节|如采集的温度值占用一个寄存器（16位），但是对采集后的原始数据要进行高低字节的交换才能生成真实的值|
        |交换寄存器顺序|如采集的振动值占用两个寄存器（32位），但是对采集后的原始数据要进行前后寄存器的交换才能生成真实值|
        |缩放因子|指缩放系数，如采集的值为100， 但是真实的值为10，因此需要缩放10倍，故缩放因子填写0.1即可。如放大10倍（即真实的值为1000），则放大因子为10|
        |采集间隔|Modbus协议是半双工协议，由网关主动请求数据，因此需要指定对数据点的采集间隔时间。单位为毫秒|
        |数据上报方式|按时上报是根据**采集间隔**指定的时间采集并上报，而变更上报指采集后的值发生变化后才会上报|

    5.  参考[单个创建设备](../../../../../cn.zh-CN/用户指南/产品与设备/创建设备/单个创建设备.md#)，添加环境监测设备。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312617_zh-CN.png)

5.  配置子设备通道。 
    1.  参考[子设备通道管理](../../../../../cn.zh-CN/用户指南/产品与设备/网关与子设备/子设备通道管理.md#)，为网关添加Modbus通道。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312621_zh-CN.png)

    2.  参考[子设备管理](../../../../../cn.zh-CN/用户指南/产品与设备/网关与子设备/子设备管理.md#)，为网关添加子设备，关联Modbus通道。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312622_zh-CN.png)

6.  创建并配置边缘实例。 边缘实例提供一种类似文件夹的管理功能，您可以通过实例的方式管理边缘相关的网关、设备，同时也可以管理规则计算、函数计算和消息路由内容。通过部署实例，将添加在实例中的资源部署至网关中。详细的操作步骤及配置方法请参考[部署边缘实例](../../../../../cn.zh-CN/用户指南/部署边缘实例.md#)。
    1.  创建边缘实例。设置实例名称，并关联[2](#)中创建的网关产品和设备。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312619_zh-CN.png)

    2.  分配环境监测设备到实例中。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312624_zh-CN.png)

    3.  分配子设备成功后，为子设备配置Modbus驱动。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312623_zh-CN.png)

        其中，从站号设置为2。

    4.  参考[设置消息路由](../../../../../cn.zh-CN/用户指南/消息路由/设置消息路由.md#)，配置边缘实例消息路由。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312625_zh-CN.png)

7.  部署边缘实例。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312626_zh-CN.png)

8.  在**设备管理** \> **设备**页面，查看设备是否在线。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21692/154831364312627_zh-CN.png)


