# C-SDK {#concept_mwn_slm_b2b .concept}

详细技术说明文档， 请访问[官方WiKi](https://github.com/aliyun/iotkit-embedded/wiki)。

## 编译配置项说明 {#section_yb1_zy5_wdb .section}

请访问[SDK下载](https://help.aliyun.com/document_detail/42648.html)，下载最新版本设备端C-SDK文档。

解压之后，打开编译配置文件`make.settings`， 根据需要编辑配置项：

```

FEATURE_MQTT_COMM_ENABLED = y # 是否打开MQTT通道的总开关
FEATURE_MQTT_DIRECT = y # 是否打开MQTT直连的分开关
FEATURE_MQTT_DIRECT_NOTLS = n # 是否打开MQTT直连无TLS的分开关
FEATURE_COAP_COMM_ENABLED = y # 是否打开CoAP通道的总开关
FEATURE_HTTP_COMM_ENABLED = y # 是否打开HTTP通道的总开关
FEATURE_SUBDEVICE_ENABLED = n # 是否打开主子设备功能的总开关
FEATURE_SUBDEVICE_STATUS = gateway # 主子设备功能所处的功能状态
FEATURE_CMP_ENABLED = y # 是否打开CMP功能的总开关
FEATURE_CMP_VIA_MQTT_DIRECT = y # CMP功能连云部分是否直接使用MQTT的开关
FEATURE_MQTT_DIRECT_NOITLS = y # 是否打开MQTT直连无ITLS的分开关 目前itls只在id2模式支持
FEATURE_DM_ENABLED = y # 是否打开DM功能的总开关
FEATURE_SERVICE_OTA_ENABLED = y # 是否打开linkit中OTA功能的分开关
FEATURE_SERVICE_COTA_ENABLED = y # 是否打开linkit中远程配置功能的分开关
```

具体含义请参见下表：

|配置选项|含义|
|----|--|
|FEATURE\_MQTT\_COMM\_ENABLED|是否使能MQTT通道功能的总开关|
|FEATURE\_MQTT\_DIRECT|是否用MQTT直连模式代替HTTPS三方认证模式做设备认证|
|FEATURE\_MQTT\_DIRECT\_NOTLS|使用MQTT直连模式做设备认证时, 是否要关闭MQTT over TLS|
|FEATURE\_COAP\_COMM\_ENABLED|是否使能CoAP通道功能的总开关|
|FEATURE\_HTTP\_COMM\_ENABLED|是否使能Https通道功能的总开关|
|FEATURE\_SUBDEVICE\_ENABLED|是否使能主子设备通道功能的总开关|
|FEATURE\_SUBDEVICE\_STATUS|主子设备功能所处的功能状态，取值有网关gateway\(gw=1\)和子设备subdevice\(gw=0\)|
|FEATURE\_CMP\_ENABLED|是否打开CMP功能的总开关，CMP: connectivity management platform|
|FEATURE\_CMP\_VIA\_CLOUD\_CONN|CMP功能连云部分选择使用CLOUD\_CONN，该开关选择具体协议：MQTT/CoAP/HTTP|
|FEATURE\_MQTT\_ID2\_AUTH|ID2功能需打开ITLS开关支持|
|FEATURE\_SERVICE\_OTA\_ENABLED|是否打开linkit中OTA功能的分开关，需打开FEATURE\_SERVICE\_OTA\_ENABLED支持|
|FEATURE\_SERVICE\_COTA\_ENABLED|是否打开linkit中COTA功能的分开关，需打开FEATURE\_SERVICE\_OTA\_ENABLED支持|
|FEATURE\_SUPPORT\_PRODUCT\_SECRET|是否打开一型一密开关，与id2互斥|

**说明：** 

-   当FEATURE\_SERVICE\_OTA\_ENABLED和FEATURE\_SERVICE\_COTA\_ENABLED的值均为y时，支持远程配置，不支持固件升级。
-   当FEATURE\_SERVICE\_OTA\_ENABLED值为y，FEATURE\_SERVICE\_COTA\_ENABLED的值为n时，支持固件升级，不支持远程配置。
-   当FEATURE\_SERVICE\_OTA\_ENABLED为n时，不支持service层级的ota，但是仍可以直接使用IOT\_OTA\_XXX来进行固件升级。

## 编译 & 运行 {#section_aht_4rv_wdb .section}

请参考[README.md](https://github.com/aliyun/iotkit-embedded/blob/master/README.md)。

## C-SDK API参考 {#section_hsx_zlm_b2b .section}

以下将为您介绍V2.0+版本C-SDK提供的功能和对应的API。

API的作用如下：

-   编写应用逻辑，以sample/\*/\*.c代码中的示例程序为准。
-   更加准确和权威的封装AT命令，以src/sdk-impl/iot\_export.h和src/sdk-impl/exports/\*.h代码中的注释为准。

执行如下命令，列出当前SDK代码提供的所有面向用户的API函数。

`cd src/sdk-impl`

`grep -o "IOT_[A-Z][_a-zA-Z]*[^_]\> *(" iot_export.h exports/*.h|sed 's!.*:\(.*\)(!\1!'|cat -n`

系统显示如下：

```

1 IOT_OpenLog
2 IOT_CloseLog
3 IOT_SetLogLevel
4 IOT_DumpMemoryStats
5 IOT_SetupConnInfo
6 IOT_SetupConnInfoSecure
7 IOT_Cloud_Connection_Init
8 IOT_Cloud_Connection_Deinit
9 IOT_Cloud_Connection_Send_Message
10 IOT_Cloud_Connection_Yield
11 IOT_CMP_Init
12 IOT_CMP_OTA_Start
13 IOT_CMP_OTA_Set_Callback
14 IOT_CMP_OTA_Get_Config
15 IOT_CMP_OTA_Request_Image
16 IOT_CMP_Register
17 IOT_CMP_Unregister
18 IOT_CMP_Send
19 IOT_CMP_Send_Sync
20 IOT_CMP_Yield
21 IOT_CMP_Deinit
22 IOT_CMP_OTA_Yield
23 IOT_CoAP_Init
24 IOT_CoAP_Deinit
25 IOT_CoAP_DeviceNameAuth
26 IOT_CoAP_Yield
27 IOT_CoAP_SendMessage
28 IOT_CoAP_GetMessagePayload
29 IOT_CoAP_GetMessageCode
30 IOT_HTTP_Init
31 IOT_HTTP_DeInit
32 IOT_HTTP_DeviceNameAuth
33 IOT_HTTP_SendMessage
34 IOT_HTTP_Disconnect
35 IOT_MQTT_Construct
36 IOT_MQTT_ConstructSecure
37 IOT_MQTT_Destroy
38 IOT_MQTT_Yield
39 IOT_MQTT_CheckStateNormal
40 IOT_MQTT_Disable_Reconnect
41 IOT_MQTT_Subscribe
42 IOT_MQTT_Unsubscribe
43 IOT_MQTT_Publish
44 IOT_OTA_Init
45 IOT_OTA_Deinit
46 IOT_OTA_ReportVersion
47 IOT_OTA_RequestImage
48 IOT_OTA_ReportProgress
49 IOT_OTA_GetConfig
50 IOT_OTA_IsFetching
51 IOT_OTA_IsFetchFinish
52 IOT_OTA_FetchYield
53 IOT_OTA_Ioctl
54 IOT_OTA_GetLastError
55 IOT_Shadow_Construct
56 IOT_Shadow_Destroy
57 IOT_Shadow_Yield
58 IOT_Shadow_RegisterAttribute
59 IOT_Shadow_DeleteAttribute
60 IOT_Shadow_PushFormat_Init
61 IOT_Shadow_PushFormat_Add
62 IOT_Shadow_PushFormat_Finalize
63 IOT_Shadow_Push
64 IOT_Shadow_Push_Async
65 IOT_Shadow_Pull
66 IOT_Gateway_Generate_Message_ID
67 IOT_Gateway_Construct
68 IOT_Gateway_Destroy
69 IOT_Subdevice_Register
70 IOT_Subdevice_Unregister
71 IOT_Subdevice_Login
72 IOT_Subdevice_Logout
73 IOT_Gateway_Get_TOPO
74 IOT_Gateway_Get_Config
75 IOT_Gateway_Publish_Found_List
76 IOT_Gateway_Yield
77 IOT_Gateway_Subscribe
78 IOT_Gateway_Unsubscribe
79 IOT_Gateway_Publish
80 IOT_Gateway_RRPC_Register
81 IOT_Gateway_RRPC_Response
82 linkkit_start
83 linkkit_end
84 linkkit_dispatch
85 linkkit_yield
86 linkkit_set_value
87 linkkit_get_value
88 linkkit_set_tsl
89 linkkit_answer_service
90 linkkit_invoke_raw_service
91 linkkit_trigger_event
92 linkkit_fota_init
93 linkkit_invoke_fota_service
94 linkkit_fota_init
95 linkkit_invoke_cota_get_config
96 linkkit_invoke_cota_service
97 linkkit_post_property
```

具体API解释如下表所示。

|序号|函数名|说明|
|--|---|--|
|**必选API**|
|1|IOT\_OpenLog|开始打印日志信息\(log\), 接受一个const char \*为入参, 表示模块名字|
|2|IOT\_CloseLog|停止打印日志信息\(log\), 入参为空|
|3|IOT\_SetLogLevel|设置打印的日志等级, 接受入参从1到5, 数字越大, 打印越详细|
|4|IOT\_DumpMemoryStats|调试函数, 打印内存的使用统计情况, 入参为1-5, 数字越大, 打印越详细|
|**CoAP功能相关**|
|1|IOT\_CoAP\_Init|CoAP实例的构造函数, 入参为iotx\_coap\_config\_t结构体, 返回创建的CoAP会话句柄|
|2|IOT\_CoAP\_Deinit|CoAP实例的摧毁函数， 入参为IOT\_CoAP\_Init\(\)所创建的句柄|
|3|IOT\_CoAP\_DeviceNameAuth|基于控制台申请的DeviceName, DeviceSecret, ProductKey做设备认证|
|4|IOT\_CoAP\_GetMessageCode|CoAP会话阶段, 从服务器的CoAP Response报文中获取Respond Code|
|5|IOT\_CoAP\_GetMessagePayload|CoAP会话阶段, 从服务器的CoAP Response报文中获取报文负载|
|6|IOT\_CoAP\_SendMessage|CoAP会话阶段, 连接已成功建立后调用, 组织一个完整的CoAP报文向服务器发送|
|7|IOT\_CoAP\_Yield|CoAP会话阶段, 连接已成功建立后调用, 检查和收取服务器对CoAP Request的回复报文|
|**云端连接Cloud Connection功能相关**|
|1|IOT\_Cloud\_Connection\_Init|云端连接实例的构造函数, 入参为iotx\_cloud\_connection\_param\_pt结构体, 返回创建的云端连接会话句柄|
|2|IOT\_Cloud\_Connection\_Deinit|云端连接实例的摧毁函数, 入参为IOT\_Cloud\_Connection\_Init\(\)所创建的句柄|
|3|IOT\_Cloud\_Connection\_Send\_Message|发送数据给云端|
|4|IOT\_Cloud\_Connection\_Yield|云端连接成功建立后，收取服务器发送的报文|
|**CMP功能相关**|
|1|IOT\_CMP\_Init|CMP实例的构造函数, 入参为iotx\_cmp\_init\_param\_pt结构体，只存在一个CMP实例|
|2|IOT\_CMP\_Register|通过CMP订阅服务|
|3|IOT\_CMP\_Unregister|通过CMP取消服务订阅|
|4|IOT\_CMP\_Send|通过CMP发送数据，可以送给云端，也可以送给本地设备|
|5|IOT\_CMP\_Send\_Sync|通过CMP同步发送数据 ，暂不支持|
|6|IOT\_CMP\_Yield|通过CMP接收数据，单线程情况下才支持|
|7|IOT\_CMP\_Deinit|CMP示例的摧毁函数|
|8|IOT\_CMP\_OTA\_Start|初始化ota功能，上报版本|
|9|IOT\_CMP\_OTA\_Set\_Callback|设置OTA回调函数|
|10|IOT\_CMP\_OTA\_Get\_Config|获取远程配置|
|11|IOT\_CMP\_OTA\_Request\_Image|获取固件|
|12|IOT\_CMP\_OTA\_Yield|通过CMP完成OTA功能|
|**MQTT功能相关**|
|1|IOT\_SetupConnInfo|MQTT连接前的准备, 基于DeviceName + DeviceSecret + ProductKey产生MQTT连接的用户名和密码等|
|2|IOT\_SetupConnInfoSecure|MQTT连接前的准备, 基于ID2 + DeviceSecret + ProductKey产生MQTT连接的用户名和密码等,ID2模式启用|
|3|IOT\_MQTT\_CheckStateNormal|MQTT连接后, 调用此函数检查长连接是否正常|
|4|IOT\_MQTT\_Construct|MQTT实例的构造函数, 入参为iotx\_mqtt\_param\_t结构体, 连接MQTT服务器, 并返回被创建句柄|
|5|IOT\_MQTT\_ConstructSecure|MQTT实例的构造函数, 入参为iotx\_mqtt\_param\_t结构体, 连接MQTT服务器, 并返回被创建句柄 ，ID2模式启用|
|6|IOT\_MQTT\_Destroy|MQTT实例的摧毁函数， 入参为IOT\_MQTT\_Construct\(\)创建的句柄|
|7|IOT\_MQTT\_Publish|MQTT会话阶段, 组织一个完整的MQTT Publish报文, 向服务端发送消息发布报文|
|8|IOT\_MQTT\_Subscribe|MQTT会话阶段, 组织一个完整的MQTT Subscribe报文, 向服务端发送订阅请求|
|9|IOT\_MQTT\_Unsubscribe|MQTT会话阶段, 组织一个完整的MQTT UnSubscribe报文, 向服务端发送取消订阅请求|
|10|IOT\_MQTT\_Yield|MQTT会话阶段, MQTT主循环函数, 内含了心跳的维持, 服务器下行报文的收取等|
|**OTA功能相关\(模组实现时的可选功能\)**|
|1|IOT\_OTA\_Init|OTA实例的构造函数， 创建一个OTA会话的句柄并返回|
|2|IOT\_OTA\_Deinit|OTA实例的摧毁函数， 销毁所有相关的数据结构|
|3|IOT\_OTA\_Ioctl|OTA实例的输入输出函数， 根据不同的命令字可以设置OTA会话的属性，或者获取OTA会话的状态|
|4|IOT\_OTA\_GetLastError|OTA会话阶段，若某个IOT\_OTA\_\*\(\)函数返回错误, 调用此接口可获得最近一次的详细错误码|
|5|IOT\_OTA\_ReportVersion|OTA会话阶段, 向服务端汇报当前的固件版本号|
|6|IOT\_OTA\_FetchYield|OTA下载阶段, 在指定的timeout时间内, 从固件服务器下载一段固件内容, 保存在入参buffer中|
|7|IOT\_OTA\_IsFetchFinish|OTA下载阶段, 判断迭代调用IOT\_OTA\_FetchYield\(\)是否已经下载完所有的固件内容|
|8|IOT\_OTA\_IsFetching|OTA下载阶段, 判断固件下载是否仍在进行中, 尚未完成全部固件内容的下载|
|9|IOT\_OTA\_ReportProgress|可选API, OTA下载阶段, 调用此函数向服务端汇报已经下载了全部固件内容的百分之多少|
|10|IOT\_OTA\_RequestImage|可选API，向服务端请求固件下载|
|11|IOT\_OTA\_GetConfig|可选API，向服务端请求远程配置|
|**HTTP功能相关**|
|1|IOT\_HTTP\_Init|Https实例的构造函数, 创建一个HTTP会话的句柄并返回|
|2|IOT\_HTTP\_DeInit|Https实例的摧毁函数, 销毁所有相关的数据结构|
|3|IOT\_HTTP\_DeviceNameAuth|基于控制台申请的DeviceName、DeviceSecret和 ProductKey做设备认证|
|4|IOT\_HTTP\_SendMessage|Https会话阶段, 组织一个完整的HTTP报文向服务器发送,并同步获取HTTP回复报文|
|5|IOT\_HTTP\_Disconnect|Https会话阶段, 关闭HTTP层面的连接, 但是仍然保持TLS层面的连接|
|**设备影子相关\(模组实现时的可选功能\)**|
|1|IOT\_Shadow\_Construct|建立一个设备影子的MQTT连接, 并返回被创建的会话句柄|
|2|IOT\_Shadow\_Destroy|摧毁一个设备影子的MQTT连接, 销毁所有相关的数据结构, 释放内存, 断开连接|
|3|IOT\_Shadow\_Pull|把服务器端被缓存的JSON数据下拉到本地, 更新本地的数据属性|
|4|IOT\_Shadow\_Push|把本地的数据属性上推到服务器缓存的JSON数据, 更新服务端的数据属性|
|5|IOT\_Shadow\_Push\_Async|和IOT\_Shadow\_Push\(\)接口类似, 但是异步的, 上推后便返回, 不等待服务端回应|
|6|IOT\_Shadow\_PushFormat\_Add|向已创建的数据类型格式中增添成员属性|
|7|IOT\_Shadow\_PushFormat\_Finalize|完成一个数据类型格式的构造过程|
|8|IOT\_Shadow\_PushFormat\_Init|开始一个数据类型格式的构造过程|
|9|IOT\_Shadow\_RegisterAttribute|创建一个数据类型注册到服务端, 注册时需要\*PushFormat\*\(\)接口创建的数据类型格式|
|10|IOT\_Shadow\_DeleteAttribute|删除一个已被成功注册的数据属性|
|11|IOT\_Shadow\_Yield|MQTT的主循环函数, 调用后接受服务端的下推消息, 更新本地的数据属性|
|**主子设备相关\(模组实现时的可选功能\)**|
|1|IOT\_Gateway\_Construct|建立一个主设备，建立MQTT连接, 并返回被创建的会话句柄|
|2|IOT\_Gateway\_Destroy|摧毁一个主设备的MQTT连接, 销毁所有相关的数据结构, 释放内存, 断开连接|
|3|IOT\_Subdevice\_Login|子设备上线，通知云端建立子设备session|
|4|IOT\_Subdevice\_Logout|子设备下线，销毁云端建立子设备session及所有相关的数据结构, 释放内存|
|5|IOT\_Gateway\_Yield|MQTT的主循环函数, 调用后接受服务端的下推消息|
|6|IOT\_Gateway\_Subscribe|通过MQTT连接向服务端发送订阅请求|
|7|IOT\_Gateway\_Unsubscribe|通过MQTT连接向服务端发送取消订阅请求|
|8|IOT\_Gateway\_Publish|通过MQTT连接服务端发送消息发布报文|
|9|IOT\_Gateway\_RRPC\_Register|注册设备的RRPC回调函数，接收云端发起的RRPC请求|
|10|IOT\_Gateway\_RRPC\_Response|对云端的RRPC请求进行应答|
|11|IOT\_Gateway\_Generate\_Message\_ID|生成消息id|
|12|IOT\_Gateway\_Get\_TOPO|向topo/get topic发送包并等待回复（TOPIC\_GET\_REPLY 回复）|
|13|IOT\_Gateway\_Get\_Config|向conifg/get topic发送包并等待回复（TOPIC\_CONFIG\_REPLY 回复）|
|14|IOT\_Gateway\_Publish\_Found\_List|发现设备列表上报|
|**linkkit\(\)**|
|1|linkkit\_start|启动 linkkit 服务，与云端建立连接并安装回调函数|
|2|linkkit\_end|停止 linkkit 服务，与云端断开连接并回收资源|
|3|linkkit\_dispatch|事件分发函数,触发 linkkit\_start 安装的回调|
|4|linkkit\_yield|linkkit 主循环函数，内含了心跳的维持，服务器下行报文的收取等。如果允许多线程，请不要调用此函数|
|5|linkkit\_set\_value|根据identifier设置物对象的 TSL 属性，如果标识符为struct类型、event output类型或者service output类型，使用’.’分隔字段，例如”identifier1.identifier2”指向特定的项|
|6|linkkit\_get\_value|根据identifier获取物对象的 TSL 属性|
|7|linkkit\_set\_tsl|从本地读取 TSL 文件，生成物的对象并添加到 linkkit 中|
|8|linkkit\_answer\_service|对云端服务请求进行回应|
|9|linkkit\_invoke\_raw\_service|向云端发送裸数据|
|10|linkkit\_trigger\_event|上报设备事件到云端|
|11|linkkit\_fota\_init|初始化 OTA-fota 服务，并安装回调函数\(需编译设置宏 SERVICE\_OTA\_ENABLED \)|
|12|linkkit\_invoke\_fota\_service|执行fota服务|
|13|linkkit\_cota\_init|初始化 OTA-cota 服务，并安装回调函数\(需编译设置宏 SERVICE\_OTA\_ENABLED SERVICE\_COTA\_ENABLED \)|
|14|linkkit\_invoke\_cota\_get\_config|设备请求远程配置|
|15|linkkit\_invoke\_cota\_service|执行cota服务|
|16|linkkit\_post\_property|上报设备属性到云端|

