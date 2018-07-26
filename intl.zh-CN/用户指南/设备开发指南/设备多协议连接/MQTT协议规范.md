# MQTT协议规范 {#concept_jfq_vjw_vdb .concept}

## 支持版本 {#section_mly_bkw_vdb .section}

目前阿里云支持MQTT标准协议接入，兼容3.1.1和3.1版本协议，具体的协议请参考 [MQTT 3.1.1](http://mqtt.org/) 和 [MQTT 3.1](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html) 协议文档。

## 与标准MQTT的区别 {#section_gl4_2kw_vdb .section}

-   支持MQTT 的 PUB、SUB、PING、PONG、CONNECT、DISCONNECT和UNSUB等报文。
-   支持cleanSession。
-   不支持will、retain msg。
-   不支持QOS2。
-   基于原生的MQTT topic上支持RRPC同步模式，服务器可以同步调用设备并获取设备回执结果。

## 安全等级 {#section_eqt_kkw_vdb .section}

支持 TLSV1、 TLSV1.1和TLSV1.2 版本的协议建立安全连接。

-   TCP通道基础＋芯片级加密（ID2硬件集成）： 安全级别高。
-   TCP通道基础＋对称加密（使用设备私钥做对称加密）：安全级别中。
-   TCP方式（数据不加密）： 安全级别低。

## topic规范 {#section_z5j_pkw_vdb .section}

默认情况下创建一个产品后，产品下的所有设备都拥有以下topic类的权限：

-   /$\{YourProductKey\}/$\{YourDeviceName\}/update pub
-   /$\{YourProductKey\}/$\{YourDeviceName\}/update/error pub
-   /$\{YourProductKey\}/$\{YourDeviceName\}/get sub
-   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/thing/\# pub&sub
-   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/\# pub&sub
-   /broadcast/$\{YourProductKey\}/\# pub&sub

每个topic规则称为topic类，topic类实行设备维度隔离。每个设备发送消息时，将deviceName替换为自己设备的deviceName，防止topic被跨设备越权，topic说明如下：

-   pub：表示数据上报到topic的权限。
-   sub：表示订阅topic的权限。
-   /$\{YourProductKey\}/$\{YourDeviceName\}/xxx类型的topic类：可以在物联网平台的控制台扩展和自定义。
-   /sys开头的topic类：属于系统约定的应用协议通信标准，不允许用户自定义的，约定的topic需要符合阿里云ALink格式数据标准。
-   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/thing/xxx类型的topic类：网关主子设备使用的topic类，用于网关场景。
-   /broadcast开头的topic类：广播类特定topic。
-   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/request/$\{yourmessageId\}：用于同步请求，服务器会对消息Id动态生成topic， 设备端可以订阅通配符。
-   /sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/request/+：收到消息后，发送pub消息到/sys/$\{YourProductKey\}/$\{YourDeviceName\}/rrpc/response/$\{yourmessageId\}，服务器可以在发送请求时，同步收到结果。

