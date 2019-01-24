# Alink协议 {#concept_pfw_hdg_cfb .concept}

物联网平台为设备端开发提供了SDK，这些SDK已封装了设备端与物联网平台的交互协议。您可以直接使用设备端SDK来进行开发。如果嵌入式环境复杂，已提供的设备端SDK不能满足您的需求，请参考本文，自行封装Alink协议数据，建立设备与物联网平台的通信。

物联网平台为设备端开发提供的各语言SDK，请参见[设备端SDK](intl.zh-CN/设备端开发指南/下载设备端SDK.md#)。

Alink协议是针对物联网开发领域设计的一种数据交换规范，数据格式是JSON，用于设备端和物联网平台的双向通信，更便捷地实现和规范了设备端和物联网平台之间的业务数据交互。

以下为您介绍Alink协议下，设备的上线流程和数据上下行原理。

## 上线流程 {#section_dqf_tzg_cfb .section}

设备上线流程，可以按照设备类型，分为直连设备接入与子设备接入。主要包括：设备注册、上线和数据上报三个流程。

直连设备接入有两种方式：

-   使用[一机一密](intl.zh-CN/设备端开发指南/设备安全认证/一机一密.md#)方式提前烧录设备证书\(ProductKey、DeviceName和DeviceSecret\)，注册设备，上线，然后上报数据。
-   使用[一型一密](intl.zh-CN/设备端开发指南/设备安全认证/一型一密.md#)动态注册提前烧录产品证书（ProductKey和ProductSecret），注册设备， 上线，然后上报数据。

子设备接入流程通过网关发起，具体接入方式有两种：

-   使用[一机一密](intl.zh-CN/设备端开发指南/设备安全认证/一机一密.md#)提前烧录设备证书\(ProductKey、DeviceName和DeviceSecret\)，子设备上报设备证书给网关，网关添加拓扑关系，复用网关的通道上报数据。
-   使用动态注册方式提前烧录ProductKey，子设备上报ProductKey和DeviceName给网关，物联网平台校验DeviceName成功后，下发DeviceSecret。子设备将获得的设备证书信息上报网关，网关添加拓扑关系，通过网关的通道上报数据。

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154832432411778_zh-CN.jpg)

## 设备上报属性或事件 {#section_isq_tzg_cfb .section}

-   透传格式（透传/自定义）数据

    ![](images/11785_zh-CN.jpeg)

    1.  设备通过透传格式数据的Topic，上报透传数据。
    2.  物联网平台通过数据解析脚本先对设备上报的数据进行解析。调用脚本中的`rawDataToProtocol`方法，将设备上报的数据转换为物联网平台标准数据格式（Alink JSON格式）。
    3.  物联网平台使用转换后的Alink JSON格式数据进行业务处理。

        **说明：** 如果配置了规则引擎数据流转，则会将数据流转到规则引擎配置的目的云产品中。

        -   规则引擎获取到的数据是经过脚本解析之后的数据。
        -   在配置规则引擎数据流转， 编写处理数据的SQL时，将Topic定义为：`/sys/{productKey}/{deviceName}/thing/event/property/post`，获取设备属性。
        -   在配置规则引擎数据流转， 编写处理数据的SQL时，将Topic定义为：`/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post`，获取设备事件。
    4.  调用数据解析脚本中的`protocolToRawData`方法，对结果数据进行格式转换，将数据解析为设备可以接收的数据格式。
    5.  推送解析后的返回结果数据给设备。
    6.  您可以通过[QueryDevicePropertyData](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyData.md#)接口查询设备上报的属性历史数据，通过[QueryDeviceEventData](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceEventData.md#)接口查询设备上报的事件历史数据。
-   非透传格式（Alink JSON）数据

    ![](images/11792_zh-CN.jpeg)

    1.  设备使用非透传格式数据的Topic，上报数据。
    2.  物联网平台进行业务处理。

        **说明：** 如果配置了规则引擎，则通过规则引擎将数据输出到规则引擎配置的目的云产品中。

        -   在配置规则引擎数据流转， 编写处理数据的SQL时，将Topic定义为：`/sys/{productKey}/{deviceName}/thing/event/property/post`，获取设备属性。
        -   在配置规则引擎数据流转， 编写处理数据的SQL时，将Topic定义为：`/sys/{productKey}/{deviceName}/thing/event/{tsl.event.identifier}/post`，获取设备事件。
    3.  物联网平台返回处理结果。
    4.  您可以通过[QueryDevicePropertyData](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDevicePropertyData.md#)接口查询设备上报的属性历史数据，通过[QueryDeviceEventData](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/QueryDeviceEventData.md#)接口查询设备上报的事件历史数据。

## 调用设备服务或设置属性 {#section_qzw_5dt_cfb .section}

-   异步服务调用或属性设置

    ![](images/11793_zh-CN.jpeg)

    1.  设置属性或调用服务。

        **说明：** 

        -   设置属性：调用[SetDeviceProperty](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/SetDeviceProperty.md#)接口为设备设置具体属性。
        -   调用服务：调用[InvokeThingService](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingService.md#)接口来异步调用服务（定义服务时，调用方式选择为异步的服务即为异步调用）。
    2.  物联网平台对您提交的参数进行校验。
    3.  物联网平台采用异步调用方式下发数据给设备，并返回调用操作结果。若没有报错，则结果中携带下发给设备的消息ID。

        **说明：** 对于透传格式（透传/自定义）数据，则会调用数据解析脚本中的`protocolToRawData`方法，对数据进行数据格式转换后，再将转换后的数据下发给设备。

    4.  设备收到数据后，进行业务处理。

        **说明：** 

        -   如果是透传格式（透传/自定义）数据，则使用透传格式数据的Topic。
        -   如果是非透传格式（Alink JSON）数据，则使用非透传格式数据的Topic。
    5.  设备完成业务处理后，返回处理结果给物联网平台。
    6.  物联网平台收到处理结果的后续操作：
        -   如果是透传格式（透传/自定义）数据，将调用数据解析脚本中的`rawDataToProtocol`方法，对设备返回的结果进行数据格式转换。
        -   如果配置了规则引擎数据流转，则将数据流转到规则引擎配置的目的Topic或云产品中。
            -   在配置规则引擎数据流转， 编写处理数据的SQL时，将Topic定义为：`/sys/{productKey}/{deviceName}/thing/downlink/reply/message`，获取异步调用的返回结果。
            -   对于透传格式（透传/自定义）数据，规则引擎获取到的数据是经过脚本解析之后的数据。
-   同步服务调用

    ![](images/11794_zh-CN.jpeg)

    1.  通过调用[InvokeThingService](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/InvokeThingService.md#)接口来调用同步服务（定义服务时，调用方式选择为同步的服务即为同步调用）。
    2.  物联网平台对您提交的参数进行校验。
    3.  使用同步调用方式，调用RRPC的Topic，下发数据给设备，物联网平台同步等待设备返回结果。

        **说明：** 对于透传格式（透传/自定义）数据，则会先调用数据解析脚本中的`protocolToRawData`方法，对数据进行数据格式转换后，再将格式转换后的数据下发给设备。

    4.  设备完成处理业务后，返回处理结果。若超时，则返回超时的错误信息。
    5.  物联网平台收到设备处理结果后，返回结果给调用者。

        **说明：** 对于透传格式（透传/自定义）数据，物联网平台会调用脚本中的`rawDataToProtocol`方法，对设备返回的结果数据进行格式转换后，再将转换后的结果数据发送给调用者。


## 拓扑关系 {#section_dgr_xxw_4fb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/21183/154832432414317_zh-CN.png)

1.  子设备连接到网关后，网关通过添加拓扑关系Topic，添加拓扑关系，物联网平台返回添加的结果。
2.  网关可以通过删除拓扑关系的Topic，来删除网关和子设备的拓扑关系。
3.  开发者可以调用[GetThingTopo](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/GetThingTopo.md#)接口来查询网关和子设备的拓扑关系。
4.  当添加拓扑关系需要第三方介入时，可以通过下面的步骤添加拓扑关系。
    1.  网关通过发现设备列表的Topic，上报发现的子设备信息。
    2.  物联网平台收到上报数据后，如果配置了规则引擎，可以通过规则引擎将数据流转到对应的云产品中，开发者可以从云产品中获取该数据。
    3.  当开发者从云产品中获取了网关发现的子设备后，可以决定是否添加与网关的拓扑关系。如果需要添加拓扑关系，可以调用[NotifyAddThingTopo](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/NotifyAddThingTopo.md#)接口通知网关发起添加拓扑关系。
    4.  物联网平台收到[NotifyAddThingTopo](../../../../../intl.zh-CN/云端开发指南/云端API参考/设备管理/NotifyAddThingTopo.md#)接口调用后，会通知添加拓扑关系的Topic将命令推送给网关。
    5.  网关收到通知添加拓扑关系的命令后，通过添加拓扑关系Topic，添加拓扑关系。

**说明：** 

-   网关通过Topic：`/sys/{productKey}/{deviceName}/thing/topo/add`，添加拓扑关系。
-   网关通过Topic：`/sys/{productKey}/{deviceName}/thing/topo/delete`，删除拓扑关系。
-   网关通过Topic：`/sys/{productKey}/{deviceName}/thing/topo/get`，获取网关和子设备的拓扑关系。
-   网关通过发现设备列表的Topic：`/sys/{productKey}/{deviceName}/thing/list/found`，上报发现的子设备信息。
-   网关通过Topic：`/sys/{productKey}/{deviceName}/thing/topo/add/notify`，通知网关设备对子设备发起添加拓扑关系

