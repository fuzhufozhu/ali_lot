# 什么是Topic {#concept_mny_tnl_vdb .concept}

物联网平台中，服务端和设备端通过 Topic 来实现消息通信。Topic是针对设备的概念，Topic类是针对产品的概念。产品的Topic类会自动映射到产品下的所有设备中，生成用于消息通信的具体设备Topic。

## 产品Topic类 {#section_exz_xv5_vdb .section}

为了方便海量设备基于海量Topic进行通信，简化授权操作，物联网平台增加了产品Topic类的概念。Topic类是一类Topic的集合。例如，产品的自定义Topic类`/${YourProductKey}/${YourDeviceName}/user/update`是具体Topic`/${YourProductKey}/device1/user/update`和`/${YourProductKey}/device2/user/update`的集合。

您创建设备后，产品的所有Topic类会自动映射到设备上。您无需单独为每个设备创建Topic。

![Topic](images/35287_zh-CN.png "Topic 自动生成示意图")

关于Topic类的说明：

-   Topic类中，以正斜线（/）进行分层，区分每个类目。其中，有两个类目为既定类目：`${YourProductKey}`表示产品的标识符ProductKey；`${YourDeviceName}`表示设备名称。
-   类目命名只能包含字母，数字和下划线（\_）。每级类目不能为空。
-   设备操作权限：**发布**表示设备可以往该Topic发布消息；**订阅**表示设备可以订阅该Topic获取消息。
-   Topic类是一个Topic模版配置，定义设备对此类Topic是否有发起发布或订阅指令的权限。编辑更新某个Topic类后，可能对产品下所有设备使用该类Topic通信产生影响。建议在设备研发阶段设计好，设备上线后不再变更Topic类。
-   变更Topic类后，对已在线设备不产生影响。但是，如果设备下线，再次上线时，Topic变更会生效，请提前在设备端上做好相应Topic变更。

    **说明：** MQTT的sub平台是一次订阅永久生效。如果需要取消订阅，需设备发起unsub请求。


## 设备Topic {#section_ozb_bw5_vdb .section}

产品的Topic类不用于通信，只是定义Topic。用于消息通信的是具体的设备Topic。

-   Topic格式和Topic类格式一致。区别在于Topic类中的变量`${YourDeviceName}`，在Topic中是具体的设备名称。
-   设备对应的Topic是从产品Topic类映射出来，根据设备名称而动态创建的。设备的具体Topic中带有设备名称（即`DeviceName`），只能被该设备用于消息通信。例如，Topic：`/${YourProductKey}/device1/user/update`归属于设备名为device1的设备，所以只能被设备device1用于发布或订阅消息，而不能被设备device2用于发布或订阅消息。

## 系统Topic和自定义Topic {#section_0e7_4fw_ppz .section}

物联网平台有两类Topic。

|类别|说明|
|:-|:-|
|系统Topic| 物联网平台预定义的Topic。

 系统Topic包含展示在控制台产品、设备详情页下的Topic和各功能使用的Topic。具体功能使用的Topic请在对应功能的文档中查看。

 例如，物模型相关的Topic一般以`/sys/`开头；固件升级相关的Topic以`/ota/`开头；设备影子的Topic以`/shadow/`开头。

 |
|自定义Topic|您可以根据业务需求，在产品的Topic类列表页，[自定义Topic类](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。|

## Topic通配符 {#section_uns_lkw_hhb .section}

物联网平台支持使用两种通配符：

|通配符|描述|
|:--|:-|
|\#|\#必须出现在Topic的最后一个类目，代表本级及下级所有类目。 例如，`/a1aycMA****/device1/user/#`表示设备Topic `/a1aycMA****/device1/user/update`和`/a1aycMA****/device1/user/update/error`。

 |
|+|代表本级所有类目。 例如，`/a1aycMA****/device1/user/+/error`，表示设备Topic `/a1aycMA****/device1/user/get/error`和`/a1aycMA****/device1/user/update/error`。

 |

通配符可用于以下两种场合：

-   使用通配符[自定义Topic](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)，实现批量订阅。
-   [编写规则引擎数据流转SQL](intl.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)时，指定数据源为一批Topic。

