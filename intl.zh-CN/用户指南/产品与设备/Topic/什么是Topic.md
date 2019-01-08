# 什么是Topic {#concept_mny_tnl_vdb .concept}

物联网平台中，服务端和设备端通过 Topic 来实现消息通信。Topic是针对设备的概念，Topic类是针对产品的概念。产品的Topic类会自动映射到产品下的所有设备中，生成用于消息通信的具体设备Topic。

## 什么是Topic类？ {#section_exz_xv5_vdb .section}

为了方便海量设备基于海量Topic进行通信，简化授权操作，物联网平台增加了产品Topic类的概念。Topic类是一类Topic的集合。例如，高级版产品的自定义Topic类`/${YourProductKey}/${YourDeviceName}/user/update`是具体Topic`/${YourProductKey}/device1/user/update`和`/${YourProductKey}/device2/user/update`的集合。

您创建产品后，物联网平台会为该产品创建系统 Topic 类。您还可以根据业务需求，自定义Topic类。基础版产品和高级版产品均支持自定义Topic 类。

-   在产品的Topic类列表页，创建[自定义Topic类](intl.zh-CN/用户指南/产品与设备/Topic/自定义Topic.md#)。
-   其他功能用到的Topic类，如固件升级等，请参考具体功能文档中的Topic相关章节进行创建。

您可以在产品详情页的Topic类列表页，查看该产品的所有Topic类。

在您创建设备后，产品Topic类会自动映射到设备上。您无需单独为每个设备授权Topic。

![](images/35287_zh-CN.png "Topic 自动生成示意图")

关于Topic类的说明：

-   Topic类中，以正斜线\(/\)进行分层，区分每个类目。其中，有两个类目为既定类目：`${YourProductKey}`表示产品的标识符ProductKey；`${YourDeviceName}`表示设备名称。
-   类目命名只能包含字母，数字和下划线\(\_\)。每级类目不能为空。
-   设备操作权限：**发布**表示设备可以往Topic发布消息；**订阅**表示设备可以从Topic订阅消息。
-   系统Topic类是由系统预定义的Topic类，不支持用户自定义，不采用`/${YourProductKey}`开头。例如，高级版中，针对物模型所提供的Topic类一般以`/sys/`开头；固件升级相关的Topic类以`/ota/`开头；设备影子的Topic类以`/shadow/`开头。

## 什么是Topic？ {#section_ozb_bw5_vdb .section}

产品的Topic类不用于通信，只是定义Topic。用于消息通信的是具体的设备Topic。

您可以在物联网平台控制台，对应的设备详情页的**Topic类列表**页，查看该设备支持的具体Topic。

-   Topic格式和Topic类格式一致。区别在于Topic类中的变量`${YourDeviceName}`，在Topic中则是具体的设备名称。
-   设备对应的Topic是从产品Topic类映射出来，根据设备名称而动态创建的。设备的具体Topic中带有设备名称（即`DeviceName`），只能被该设备用于消息通信。例如，Topic：`/${YourProductKey}/device1/user/update`归属于设备名为device1的设备，所以只能被设备 device1 用于发布、订阅消息，而不能被设备 device2 用于发布、订阅消息。
-   使用规则引擎来转发设备数据，需配置相关Topic。在[设置规则引擎](intl.zh-CN/用户指南/规则引擎/设置数据流转规则.md#)时，配置的Topic中可使用通配符，且同一个类目中只能出现一个通配符。

    |通配符|描述|
    |:--|:-|
    |\#|这个通配符必须出现在Topic的最后一个类目，代表本级及下级所有类目。例如， Topic：`/${YourProductKey}/device1/user/#`，可以代表`/${YourProductKey}/device1//user/update`和`/${YourProductKey}/device1/user/update/error`。|
    |+|代表本级所有类目。例如，Topic：`/${YourProductKey}/+/user/update`，可以代表`/${YourProductKey}/device1/user/update`和`/${YourProductKey}/device2/user/update`。|


