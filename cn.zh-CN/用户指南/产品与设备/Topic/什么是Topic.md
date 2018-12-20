# 什么是Topic {#concept_mny_tnl_vdb .concept}

物联网平台中，服务端和设备端通过 Topic 来实现消息通信。Topic是针对设备的概念，Topic类是针对产品的概念。

## 什么是Topic类？ {#section_exz_xv5_vdb .section}

为了方便海量设备基于海量 Topic 进行通信，简化授权操作，物联网平台增加了 Topic 类的概念。您创建产品后，物联网平台会为该产品自动创建默认的 Topic 类。并且，在您创建设备后，会自动将产品 Topic 类映射到设备上。您无需单独为每个设备授权 Topic。

![](images/2678_zh-CN.png "Topic 自动生成示意图")

在您创建产品后，物联网平台会自动为您的产品生成一些标准的 Topic 类。您可以在产品的消息通信页面，查看该产品的所有 Topic 类。

关于 Topic 类的说明：

-   Topic类是一类 Topic 的集合。例如，Topic 类：`/${YourProductKey}/${YourDeviceName}/update`是具体 Topic：`/${YourProductKey}/device1/update`和`/${YourProductKey}/device2/update`的集合。
-   Topic类中必须以正斜线\(/\)进行分层，区分每个类目。其中，有两个类目为既定类目：`${YourProductKey}`表示产品的标识符 ProductKey；`${YourDeviceName}`表示设备名称。
-   类目命名只能包含字母，数字和下划线\(\_\)。每级类目不能为空。
-   设备操作权限：**发布**表示设备可以往 Topic 发布消息；**订阅**表示设备可以从 Topic 订阅消息。
-   基础版产品和高级版产品都支持自定义 Topic 类。您可以根据业务需求，通过自定义 Topic 类灵活地进行消息通信。
-   系统 Topic 类是由系统预定义的 Topic 类，不支持用户自定义，不采用`/${YourProductKey}`开头。例如，高级版中，针对物模型所提供的 Topic 类一般以`/sys/`开头；固件升级相关的Topic类以`/ota/`开头；设备影子的 Topic 类以`/shadow/`开头。

您可以登录IoT控制台，在对应产品详情页**消息通信**界面，查看该产品支持的Topic类。消息通信系统Topic已默认展示在页面上，自定义Topic请参考[文档](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)操作，其他功能用到的Topic类，请参考具体功能文档。

## 什么是Topic？ {#section_ozb_bw5_vdb .section}

产品的 Topic 类不用于通信，只是定义 Topic。用于消息通信的是具体的 Topic。

-   Topic 格式和Topic 类格式一致。区别在于 Topic 类中的变量`${YourDeviceName}`，在 Topic 中则是具体的设备名称。
-   设备对应的 Topic 是从产品 Topic 类映射出来，根据设备名称而动态创建的。设备的具体 Topic 中带有设备名称（即`DeviceName`），只能被该设备用于 Pub/Sub 通信。例如，Topic：`/${YourProductKey}/device1/update`归属于设备名为device1的设备，所以只能被设备 device1 用于发布、订阅消息，而不能被设备 device2 用于发布订阅消息。
-   在配置规则引擎时，配置的 Topic 中可使用通配符，且同一个类目中只能出现一个通配符。

    |通配符|描述|
    |:--|:-|
    |\#|这个通配符必须出现在 Topic 的最后一个类目，代表本级及下级所有类目。例如， Topic：`/YourProductKey/device1/#`，可以代表`/YourProductKey/device1/update`和`YourProductKey/device1/update/error`。|
    |+|代表本级所有类目。例如，Topic：`/YourProductKey/+/update`，可以代表`/${YourProductKey}/device1/update`和`/${YourProductKey}/device2/update`。|


您可以登录IoT控制台，在对应设备详情页**消息通信**界面，查看该设备支持的具体Topic。消息通信系统Topic已默认展示在页面上，自定义Topic请参考[文档](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)操作，其他功能用到的Topic，请参考具体功能文档。

