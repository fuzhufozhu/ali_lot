# Alink协议 {#concept_pfw_hdg_cfb .concept}

物联网平台为设备端开发提供了SDK，这些SDK已封装了设备端与云端的交互协议。您可以直接使用设备端SDK来进行开发。如果嵌入式环境复杂，已提供的设备端SDK不能满足您的需求，请参考本文，自行封装Alink协议数据，建立设备与云端的通信。

Alink协议是针对物联网开发领域设计的一种数据交换规范，数据格式是JSON，用于设备端和云端的双向通信，更便捷地实现和规范了设备端和云端之间的业务数据交互。

以下为您介绍Alink协议下，设备的上线流程和数据上下行原理。

## 上线流程 {#section_dqf_tzg_cfb .section}

设备上线流程，可以按照设备类型，分为直连设备接入与子设备接入。主要包括：设备注册、上线和数据上报三个流程。

直连设备接入有两种方式：

-   使用[一机一密](intl.zh-CN/设备端开发指南/C-SDK/设备安全认证/一机一密.md#)方式提前烧录三元组，注册设备，上线，然后上报数据。
-   使用[一型一密](intl.zh-CN/设备端开发指南/C-SDK/设备安全认证/一型一密.md#)动态注册提前烧录产品证书（ProductKey和ProductSecret），注册设备， 上线，然后上报数据。

子设备接入流程通过网关发起，具体接入方式有两种：

-   使用[一机一密](intl.zh-CN/设备端开发指南/C-SDK/设备安全认证/一机一密.md#)提前烧录三元组，子设备上报三元组给网关，网关添加拓扑关系，复用网关的通道上报数据。
-   使用动态注册方式提前烧录ProductKey，子设备上报ProductKey和DeviceName给网关，云端校验DeviceName成功后，下发DeviceSecret。子设备将获得的三元组信息上报网关，网关添加拓扑关系，通过网关的通道上报数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154098979411778_zh-CN.jpg)

## 设备上报属性或事件 {#section_isq_tzg_cfb .section}

-   透传格式（透传/自定义）数据

    ![](images/11785_zh-CN.jpeg)

    1.  使用透传格式的Topic，设备上报透传数据。
    2.  云端通过脚本先对设备上报的数据进行解析。调用脚本中的rawDataToProtocal方法将设备上报的数据转换为IoT平台标准数据格式（Alink JSON格式）。
    3.  使用转换后的Alink JSON格式数据进行业务处理。

        如果配置了规则引擎，则通过规则引擎将数据输出到规则引擎配置的目的云产品中。

    4.  对于云端返回的结果，通过脚本进行解析。
    5.  转换后的返回结果推送给设备。
    6.  开发者可以通过QueryDevicePropertyData接口查询设备上报的属性历史数据，通过QueryDeviceEventData接口查询设备上报的事件历史数据。
    **说明：** 

    -   规则引擎获取到的数据是经过脚本解析之后的数据。
    -   可以在规则引擎中使用Topic：/sys/\{productKey\}/\{deviceName\}/thing/event/property/post，获取设备属性
    -   可以在规则引擎中使用Topic：/sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post，获取设备事件。
-   非透传格式（Alink JSON）数据

    ![](images/11792_zh-CN.jpeg)

    1.  使用非透传格式的Topic，设备上报数据。
    2.  云端进行业务处理。

        如果配置了规则引擎，则通过规则引擎将数据输出到规则引擎配置的目的云产品中。

    3.  云端返回结果。
    4.  开发者可以通过QueryDevicePropertyData接口查询设备上报的属性历史数据，通过QueryDeviceEventData接口查询设备上报的事件历史数据。
    **说明：** 

    -   可以在规则引擎中使用Topic：/sys/\{productKey\}/\{deviceName\}/thing/event/property/post，获取设备属性。
    -   可以在规则引擎中使用Topic：/sys/\{productKey\}/\{deviceName\}/thing/event/\{tsl.event.identifier\}/post，获取设备事件。

## 设备服务调用或属性设置 {#section_qzw_5dt_cfb .section}

-   异步服务调用或属性设置

    ![](images/11793_zh-CN.jpeg)

    1.  设置设备属性或者异步调用服务。
    2.  对参数进行校验。
    3.  IoT采用异步调用进行业务处理并返回调用结果，若没有报错，则结果中携带下发给设备的消息id。

        如果是透传格式（透传/自定义）数据，通过脚本解析，调用脚本中的protocalToRawData方法，对下发给设备的数据进行数据格式转换。

    4.  下发数据给设备，设备异步处理结果。
        -   如果是透传格式（透传/自定义）数据，则使用透传格式Topic。
        -   如果是非透传格式（Alink JSON）数据，则使用非透传格式Topic。
    5.  设备处理完成业务后，返回处理结果。
        -   如果是透传格式（透传/自定义）数据，通过脚本解析，调用脚本中的rawDataToProtocal方法，对设备返回的结果进行数据格式转换。
        -   如果配置了规则引擎，则通过规则引擎将数据输出到规则引擎配置的目的云产品中。
    **说明：** 

    -   可以在规则引擎中， 使用Topic：/sys/\{productKey\}/\{deviceName\}/thing/downlink/reply/message，获取异步调用的返回结果。
    -   透传格式（透传/自定义）规则引擎获取到的数据是经过脚本解析之后的数据。
-   同步服务调用或属性设置

    ![](images/11794_zh-CN.jpeg)

    1.  调用同步服务。
    2.  对参数进行校验。

        如果是透传格式（透传/自定义）数据，通过脚本解析，调用脚本中的protocalToRawData方法，对下发给设备的数据进行数据格式转换。

    3.  使用同步调用方式，调用RRPC的Topic，下发数据给设备，云端同步等待设备返回结果。
    4.  设备处理完成业务后，返回处理结果。若超时则返回超时的错误信息。

        如果是透传格式（透传/自定义）数据，通过脚本解析，调用脚本中的rawDataToProtocal方法，对设备返回的结果进行数据格式转换。

    5.  返回结果

## 拓扑关系 {#section_dgr_xxw_4fb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154098979414317_zh-CN.png)

1.  子设备连接到网关后，网关通过添加拓扑关系Topic，添加拓扑关系，云端返回添加的结果。
2.  网关可以通过删除拓扑关系的Topic，来删除网关和子设备的拓扑关系。
3.  开发者可以调用GetThingTopo接口来查询网关和子设备的拓扑关系。
4.  当添加拓扑关系需要第三方介入时，可以通过下面的步骤添加拓扑关系。
    1.  网关通过发现设备列表的Topic，上报发现的子设备信息。
    2.  云端收到上报数据后，如果配置了规则引擎，可以通过规则引擎将数据流转到对应的云产品中，开发者可以从云产品中获取该数据。
    3.  当开发者从云产品中获取了网关发现的子设备后，可以决定是否添加与网关的拓扑关系。如果需要添加拓扑关系，可以调用NotifyAddThingTopo接口，通知网关发起添加拓扑关系。
    4.  云端收到NotifyAddThingTopo接口调用后，会通知添加拓扑关系的Topic将命令推送给网关。
    5.  网关收到通知添加拓扑关系的命令后，通过添加拓扑关系Topic，添加拓扑关系。

**说明：** 

-   网关通过Topic：/sys/\{productKey\}/\{deviceName\}/thing/topo/add，添加拓扑关系。
-   网关通过Topic：/sys/\{productKey\}/\{deviceName\}/thing/topo/delete，删除拓扑关系。
-   网关通过Topic：/sys/\{productKey\}/\{deviceName\}/thing/topo/get，获取网关和子设备的拓扑关系。
-   网关通过发现设备列表的Topic：/sys/\{productKey\}/\{deviceName\}/thing/list/found，上报发现的子设备信息。
-   网关通过Topic：/sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify，通知网关设备对子设备发起添加拓扑关系

