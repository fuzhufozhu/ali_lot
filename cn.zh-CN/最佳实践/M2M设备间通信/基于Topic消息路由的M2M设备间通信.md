# 基于Topic消息路由的M2M设备间通信 {#task_q45_glr_ydb .task}

本文以智能灯和手机App连接为例，基于物联网平台的Topic消息路由服务，构建一个M2M设备间通信架构。

智能灯控制流程如下图：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/13912/15586837264210_zh-CN.png)

1.  在物联网平台，为智能灯设备创建产品和设备，定义功能等。请参见文档[创建产品](../../../../cn.zh-CN/用户指南/产品与设备/创建产品.md#)、[批量创建设备](../../../../cn.zh-CN/用户指南/产品与设备/创建设备/批量创建设备.md#)、[新增物模型](../../../../cn.zh-CN/用户指南/产品与设备/物模型/新增物模型.md#)。 本示例中，智能灯的ProductKey为al123456789；DeviceName为light。
2.  开发智能灯设备端。 本示例中，设备与物联网平台间的通信协议为MQTT。 设备端SDK开发详情，请参见[设备端Link Kit SDK文档](https://help.aliyun.com/product/93051.html)。
3.  在物联网平台，为手机App注册产品和设备。 上图示例中，手机App的ProductKey为al987654321；DeviceName为ControlApp。 当手机App用户注册登录时，服务器将App设备信息发送给手机App。手机App可以作为一个设备连接到物联网平台。
4.  开发手机App。 本示例中，手机App与物联网平台间的通信协议为HTTPS。
5.  调用云端接口[CreateTopicRouteTable](../../../../cn.zh-CN/云端开发指南/云端API参考/Topic管理/CreateTopicRouteTable.md#)，创建App Topic与智能灯Topic之间的消息路由关系。 
    -   将入参SrcTopic指定为App的Topic：`/al987654321/ControlApp/user/update`。
    -   将入参DstTopics指定为智能灯的Topic：`/al123456789/light/user/set`。
6.  手机App用户通过扫码，将App与智能灯绑定。 当App向服务器发送绑定设备的请求后，服务器将返回绑定成功的智能灯设备名称deviceName。本示例中，智能灯设备名称为light。
7.  通过App发送控制指令。 

    1.  手机App发送指令到Topic：`/al987654321/ControlApp/user/update`。指令为JSON格式的数据。

    2.  物联网平台根据已定义的Topic路由关系，将指令信息路由到智能灯设备的Topic：`/al123456789/light/user/set`。
    3.  智能灯设备接收到指令后，执行相关操作。
    **说明：** 可配置手机App调用云端接口[DeleteTopicRouteTable](../../../../cn.zh-CN/云端开发指南/云端API参考/Topic管理/DeleteTopicRouteTable.md#)，删除消息路由关系。路由关系删除后，该手机App将不再控制该智能灯。


