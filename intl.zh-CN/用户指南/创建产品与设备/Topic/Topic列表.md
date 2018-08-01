# Topic列表 {#concept_ogz_vnl_vdb .concept}

物联网平台的Topic分为系统定义的Topic和自定义的Topic，您可以直接使用系统定义的Topic或参见[自定义Topic](intl.zh-CN/用户指南/创建产品与设备/Topic/自定义Topic.md#)添加更多自定义Topic类。本文主要列出了系统定义的Topic。

## 基础版默认Topic类 {#section_c1y_pb1_wdb .section}

创建基础版产品之后，系统自动创建三个默认的用户Topic类。

-   `/${YourProductKey}/${YourDeviceName}/update`：用于设备上报数据。设备操作权限：发布。
-   `/${YourProductKey}/${YourDeviceName}/update/error`：用于设备上报错误。设备操作权限：发布。
-   `/${YourProductKey}/${YourDeviceName}/get`：用于设备获取云端数据。设备操作权限：订阅。

## 高级版默认Topic类 {#section_cf2_l21_wdb .section}

高级版产品创建后，您可以使用 SDK 实现消息通信，或者使用Pub 或 Sub 相关的系统 Topic 实现设备数据的上下行通信。

高级版默认Topic类分为 Alink Topic 类和 透传 Topic 类。Alink Topic 类是设备采用 Alink JSON 进行消息通信时使用的 Topic 类。透传 Topic 类是设备采用透传/自定义格式进行消息通信时使用的Topic类。

Alink Topic 类：

-   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/property/post`：用于设备上报属性。设备操作权限：发布。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/service/property/set`：用于设置设备属性。设备操作权限：订阅。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/event/{tsl.event.identifer}/post`：用于设备上报事件。设备操作权限：发布。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/service/{tsl.service.identifer}`：用于调用设备服务。设备操作权限：订阅。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/deviceinfo/update`：用于设备上报标签。设备操作权限：发布。

透传 Topic 类：

-   `/sys/${YourProductKey}/${YourDeviceName}/thing/model/up_raw`：设备透传数据上报，用于设备上报属性、事件和扩展信息的数据。设备操作权限：发布。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/model/down_raw`：设备透传数据下行，用于获取设备属性、设置设备属性和调用设备服务。设备操作权限：订阅。

## 设备影子相关Topic类 {#section_ofn_t31_wdb .section}

设备影子提供系统 Topic 类，主要用于基础版产品的影子更新。

-   `/shadow/update/${YourProductKey}/${YourDeviceName}`：用于更新设备影子。设备和应用程序操作权限：发布。
-   `/shadow/get/${YourProductKey}/${YourDeviceName}`：用于获取设备影子。设备操作权限：订阅。

## 远程配置相关Topic类 {#section_blq_v1b_wdb .section}

远程配置提供系统Topic类，用于给设备下发配置文件。

-   `/sys/${YourProductKey}/${YourDeviceName}/thing/config/push`：用于设备接收云端推送的配置信息。设备操作权限：订阅。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/config/get`：用于设备主动请求更新配置。设备操作权限：发布。
-   `/sys/${YourProductKey}/${YourDeviceName}/thing/config/get_reply`：用于设备主动请求更新配置，接收云端返回的配置信息。设备操作权限：订阅。

## 固件升级相关Topic类 {#section_rcj_3bb_wdb .section}

固件升级提供系统Topic类，用于设备上报固件版本和接收升级通知。

-   `/ota/device/inform/${YourProductKey}/${YourDeviceName}`：用于设备端上报固件版本给云端。设备操作权限：发布。
-   `/ota/device/upgrade/${YourProductKey}/${YourDeviceName}`：用于设备端接收云端固件升级通知。设备操作权限：订阅。
-   `/ota/device/progress/${YourProductKey}/${YourDeviceName}`：用于设备端上报固件升级进度。设备操作权限：发布。
-   `/ota/device/request/${YourProductKey}/${YourDeviceName}`：设备端请求是否固件升级。设备操作权限：发布。

## 设备广播Topic类 {#section_gx2_3cb_wdb .section}

设备广播提供系统Topic类，但是可以自定义广播设备范围。

`/broadcast/${YourProductKey}/+`：用于设备接收广播消息。设备操作权限：订阅。通配符（+）可自定义为广播的设备范围。

## RRPC通信相关Topic类 {#section_o43_pkc_b2b .section}

IoT Hub基于开源协议MQTT封装了同步的通信模式，服务端下发指令给设备可以同步得到设备端的响应。

-   `/sys/${YourProductKey}/${YourDeviceName}/rrpc/response/${messageId}`：用于设备响应RRPC请求。设备操作权限：发布。
-   `/sys/${YourProductKey}/${YourDeviceName}/rrpc/request/+`：用于设备订阅RRPC请求。设备操作权限：订阅。

