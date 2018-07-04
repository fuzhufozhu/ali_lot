# C SDK {#concept_mwn_slm_b2b .concept}

For more information, see [the Alibaba Cloud Wiki page](https://github.com/aliyun/iotkit-embedded/wiki).

## Configuration items {#section_yb1_zy5_wdb .section}

For more information about downloading the latest C SDK, see [Download device SDKs](https://help.aliyun.com/document_detail/42648.html).

Extract the downloaded SDK, open the `make.settings` configuration file, and then configure the following parameters:

```

FEATURE_MQTT_COMM_ENABLED = y # Whether Message Queuing Telemetry Transport (MQTT)-based connections are enabled
FEATURE_MQTT_DIRECT = y # Whether MQTT-based direct connections are enabled
FEATURE_MQTT_DIRECT_NOTLS = n # Whether MQTT-based direct connections without using Transport Layer Security (TLS) are enabled
FEATURE_COAP_COMM_ENABLED = y # Whether Constrained Application Protocol (CoAP)-based connections are enabled
FEATURE_HTTP_COMM_ENABLED = y # Whether Hypertext Transfer Protocol (HTTP)-based connections are enabled
FEATURE_SUBDEVICE_ENABLED = n # Whether the gateway and sub-device feature is enabled
FEATURE_SUBDEVICE_STATUS = gateway # Status of the gateway and sub-device feature
FEATURE_CMP_ENABLED = y # Whether the connection to connectivity management platform (CMP) is enabled
FEATURE_CMP_VIA_MQTT_DIRECT = y # Whether the MQTT-based connection between CMP and IoT Platform is enabled
FEATURE_MQTT_DIRECT_NOITLS = y # Whether MQTT-based direct connections without using iTLS are enabled. Currently, only ID² authentication supports iTLS.
FEATURE_DM_ENABLED = y # Whether the domain model (DM) feature is enabled
FEATURE_SERVICE_OTA_ENABLED = y # Whether the LinkIt Over-the-Air Technology (OTA) is enabled
FEATURE_SERVICE_COTA_ENABLED = y # Whether the LinkIt remote configuration is enabled
```

For more information about these features, see 

|Parameter|Description|
|---------|-----------|
|FEATURE\_MQTT\_COMM\_ENABLED|Whether MQTT-based connections are enabled|
|FEATURE\_MQTT\_DIRECT|Whether the MQTT-based direct connection instead of HTTP Secure \(HTTPS\) third-party device authentication is enabled.|
|FEATURE\_MQTT\_DIRECT\_NOTLS|Whether MQTT over TLS is enabled during the MQTT-based direct connection|
|FEATURE\_COAP\_COMM\_ENABLED|Whether CoAP-based connection is enabled|
|FEATURE\_HTTP\_COMM\_ENABLED|Wether HTTPS-based connection is enabled|
|FEATURE\_SUBDEVICE\_ENABLED|Wether the gateway and subdevice feature is enabled.|
|FEATURE\_SUBDEVICE\_STATUS|Status of the gateway and sub-device features. Values include: gateway\(gw=1\) and subdevice\(gw=0\).|
|FEATURE\_CMP\_ENABLED|Whether the connection to CMP is enabled.|
|FEATURE\_CMP\_VIA\_CLOUD\_CONN |CLOUD\_CONN in the connection between CMP and IoT Platform. Values include: MQTT, CoAP, and HTTP.|
|FEATURE\_MQTT\_ID2\_AUTH|To use ID² authentication, you need to enable iTLS.|
|FEATURE\_SERVICE\_OTA\_ENABLED|Whether LinkIt OTA is enabled. You need to enable FEATURE\_SERVICE\_OTA\_ENABLED.|
|FEATURE\_SERVICE\_COTA\_ENABLED|Whether LinkIt Cota is enabled. You need to enable FEATURE\_SERVICE\_OTA\_ENABLED.|
|FEATURE\_SUPPORT\_PRODUCT\_SECRET|Whether unique certificate per product is used to authenticate products. This parameter cannot be enabled at the same time as ID².|

**Note:** 

-   When FEATURE\_SERVICE\_OTA\_ENABLED and FEATURE\_SERVICE\_COTA\_ENABLED are set to y, the system supports remote configuration, but does not support firmware upgrades.
-   When FEATURE\_SERVICE\_OTA\_ENABLED is set to y and FEATURE\_SERVICE\_COTA\_ENABLED is set to n, the system supports firmware upgrades, but does not support remote configuration.
-   When FEATURE\_SERVICE\_OTA\_ENABLED is set to n, the system does not support service-level OTA. However, you can use IOT\_OTA\_XXX to upgrade firmware.

## Compile & run functions {#section_aht_4rv_wdb .section}

For more information, see [README.md](https://github.com/aliyun/iotkit-embedded/blob/master/README.md).

## C SDK APIs {#section_hsx_zlm_b2b .section}

This topic describes the features and APIs provided in the C SDK version 2.0 and later.

The APIs are as follows:

-   For application logic, see the examples in sample/\*/\*.c.
-   For AT command details, see the comments in src/sdk-impl/iot\_export.h and src/sdk-impl/exports/\*.h.

You can run the following commands to list all API functions that are provided by the SDK.

`cd src/sdk-impl`

`grep -o "IOT_[A-Z][_a-zA-Z]*[^_]\> *(" iot_export.h exports/*.h|sed 's!. *:\(. *\)(! \1!' |cat -n`

The API functions are:

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

API details are shown in the following table.

|No.|Function|Description|
|---|--------|-----------|
|**Required APIs**|
|1|IOT\_OpenLog|Prints logs. The input parameter \(const char \*\) is a pointer to a const char that indicates a module name.|
|2|IOT\_CloseLog|Stops log printing. The input parameter is null.|
|3|IOT\_SetLogLevel|Sets the log printing level. The input parameter is an integer from 1 to 5. The larger the number, the more details are printed. |
|4|IOT\_DumpMemoryStats|A debugging function that prints memory usage statistics. The input parameter is an integer from 1 to 5. The larger the number, the more details are printed.|
|**CoAP functions**|
|1|IOT\_CoAP\_Init|CoAP constructor. The input parameter is an iotx\_coap\_config\_t struct type, and the output is a CoAP session handle.|
|2|IOT\_CoAP\_Deinit|CoAP destructor. The input parameter is a CoAP session handle that is returned by IOT\_CoAP\_Init\(\).|
|3|IOT\_CoAP\_DeviceNameAuth|Performs device authentication based on the DeviceName, DeviceSecret, and ProductKey.|
|4|IOT\_CoAP\_GetMessageCode|Obtains the respond code from the server's CoAP response packets.|
|5|IOT\_CoAP\_GetMessagePayload|Obtains the payloads from the server's CoAP response packets.|
|6|IOT\_CoAP\_SendMessage|Creates a complete CoAP packet to send to the server. Call this function after a CoAP connection has been established.|
|7|IOT\_CoAP\_Yield|Receives and checks the server's response packet to a CoAP request. Call this function after a CoAP connection has been established.|
|**Cloud connection functions**|
|1|IOT\_Cloud\_Connection\_Init|Cloud connection constructor. The input parameter is an iotx\_cloud\_connection\_param\_pt struct type,  and the output is a cloud connection session handle.|
|2|IOT\_Cloud\_Connection\_Deinit|Cloud connection destructor. The input parameter is a cloud connection session handle returned by IOT\_Cloud\_Connection\_Init\(\).|
|3|IOT\_Cloud\_Connection\_Send\_Message|Sends data to IoT Platform.|
|4|IOT\_Cloud\_Connection\_Yield|Receives the packets sent from the server. Call this function after a cloud connection has been established.|
|**CMP functions**|
|1|IOT\_CMP\_Init|CMP constructor. The input parameter is an iotx\_cmp\_init\_param\_pt struct type. Only one CMP instance can exist globally.|
|2|IOT\_CMP\_Register|Subscribes to a service through CMP.|
|3|IOT\_CMP\_Unregister|Unsubscribes from a service through CMP.|
|4|IOT\_CMP\_Send|Sends data to IoT Platform or devices through CMP.|
|5|IOT\_CMP\_Send\_Sync|Synchronously sends data to IoT platform or devices through CMP. This function is currently not available.|
|6|IOT\_CMP\_Yield|Receives data through CMP. This function is only available when a single thread is used.|
|7|IOT\_CMP\_Deinit|CMP destructor.|
|8|IOT\_CMP\_OTA\_Start|Initializes the OTA function and reports the version number.|
|9|IOT\_CMP\_OTA\_Set\_Callback|Sets the OTA callback.|
|10|IOT\_CMP\_OTA\_Get\_Config|Gets the remote configurations.|
|11|IOT\_CMP\_OTA\_Request\_Image|Gets the firmware.|
|12|IOT\_CMP\_OTA\_Yield|Implements the OTA update through CMP.|
|**MQTT functions**|
|1|IOT\_SetupConnInfo|Creates the username and password used for the MQTT connection based on the DeviceName, DeviceSecret, and ProductKey.  Call this function before you establish MQTT connections.|
|2|IOT\_SetupConnInfoSecure|Creates the username and password used for the MQTT connection based on the ID2, DeviceSecret, and ProductKey.  Call this function before you establish MQTT connections.|
|3|IOT\_MQTT\_CheckStateNormal|Checks whether the persistent connection is normal. Call this function after an MQTT connection has been established.|
|4|IOT\_MQTT\_Construct|MQTT constructor. The input parameter is an iotx\_mqtt\_param\_t struct type,  and the output is an MQTT session handle.|
|5|IOT\_MQTT\_ConstructSecure|MQTT constructor. The input parameter is an iotx\_mqtt\_param\_t struct type, and the output is an MQTT session handle. The ID2 mode is enabled.|
|6|IOT\_MQTT\_Destroy|MQTT destructor. The input parameter is an MQTT session handle returned by IOT\_MQTT\_Construct\(\).|
|7|IOT\_MQTT\_Publish|Creates a complete MQTT Publish packet and sends this packet to the server.|
|8|IOT\_MQTT\_Subscribe|Creates a complete MQTT Subscribe packet and sends this packet to the server.|
|9|IOT\_MQTT\_Unsubscribe|Creates a complete MQTT UnSubscribe packet and sends this packet to the server.|
|10|IOT\_MQTT\_Yield|The main cyclic function that includes the keep-alive timer and receives the packets from the server.|
|**OTA functions \(Optional\)**|
|1|IOT\_OTA\_Init|OTA constructor. Creates and returns an OTA session handle.|
|2|IOT\_OTA\_Deinit|OTA destructor. Destroys relevant data structures.|
|3|IOT\_OTA\_Ioctl|OTA input and output functions. Use this function to set the properties of OTA sessions, or to get the statuses of OTA sessions.|
|4|IOT\_OTA\_GetLastError|When an IOT\_OTA\_\*\(\) function returns an error, call this function to get the most recent error code.|
|5|IOT\_OTA\_ReportVersion|Reports the version number of the specified firmware to the server.|
|6|IOT\_OTA\_FetchYield|Downloads a piece of firmware from the firmware server during the specified timeout period and stores the firmware in the buffer. Specify the buffer as the input parameter.|
|7|IOT\_OTA\_IsFetchFinish|Checks whether IOT\_OTA\_FetchYield\(\) has downloaded the complete firmware. IOT\_OTA\_FetchYield\(\) is iteratively called.|
|8|IOT\_OTA\_IsFetching|Checks whether the firmware download is still in progress.|
|9|IOT\_OTA\_ReportProgress|Reports the percentage of the firmware that has been downloaded to the server. This function is optional.|
|10|IOT\_OTA\_RequestImage|Sends a firmware download request to the server. This function is optional.|
|11|IOT\_OTA\_GetConfig|Sends a remote configuration request to the server. This function is optional.|
|**HTTP functions**|
|1|IOT\_HTTP\_Init|Https constructor. Creates and returns an HTTP session handle.|
|2|IOT\_HTTP\_DeInit|Https destructor. Destroys relevant data structures.|
|3|IOT\_HTTP\_DeviceNameAuth|Performs device authentication based on the DeviceName, DeviceSecret, and ProductKey.|
|4|IOT\_HTTP\_SendMessage|Creates an HTTP packet, sends this packet to the server, and gets the response packet from the server.|
|5|IOT\_HTTP\_Disconnect|Disconnects the HTTP connection, but keeps the TLS connection.|
|**Device shadow functions \(Optional\)**|
|1|IOT\_Shadow\_Construct|Establishes an MQTT connection to a device shadow and returns the created session handle.|
|2|IOT\_Shadow\_Destroy|Disconnects an MQTT connection from a device shadow, destroys relevant data structures, and releases the memory.|
|3|IOT\_Shadow\_Pull|Pulls cached JSON data from the server to update the local data.|
|4|IOT\_Shadow\_Push|Pushes local data to the server to update the cached JSON data.|
|5|IOT\_Shadow\_Push\_Async|Similar to IOT\_Shadow\_Push\(\). This function is asynchronous and returns without waiting for the response from the server. |
|6|IOT\_Shadow\_PushFormat\_Add|Adds attributes to the existing data type formats.|
|7|IOT\_Shadow\_PushFormat\_Finalize|Finishes the construction of a data type format.|
|8|IOT\_Shadow\_PushFormat\_Init|Starts the construction of a data type format.|
|9|IOT\_Shadow\_RegisterAttribute|Creates a data type and registers it to the server. You must use the data type format created by \*PushFormat\*\(\) when registering the data type.|
|10|IOT\_Shadow\_DeleteAttribute|Deletes a data type attribute that has been registered.|
|11|IOT\_Shadow\_Yield|The main cyclic function that receives messages from the server and updates local data attributes.|
|**Gateway and sub-device functions \(Optional\)**|
|1|IOT\_Gateway\_Construct|Establishes an MQTT connection to a gateway, and returns the created session handle.|
|2|IOT\_Gateway\_Destroy|Disconnects an MQTT connection from a gateway, destroys relevant data structures, and releases the memory.|
|3|IOT\_Subdevice\_Login|When a sub-device is logged on, notifies IoT Platform to establish a sub-device session.|
|4|IOT\_Subdevice\_Logout|When a sub-device is logged out, destroys the sub-device sessions and relevant data structures, and releases the memory.|
|5|IOT\_Gateway\_Yield|The main cyclic function that receives messages from the server.|
|6|IOT\_Gateway\_Subscribe|Creates an MQTT Subscribe packet and sends this packet to the server.|
|7|IOT\_Gateway\_Unsubscribe|Creates an MQTT UnSubscribe packet and sends this packet to the server.|
|8|IOT\_Gateway\_Publish|Creates an MQTT Publish packet and sends this packet to the server.|
|9|IOT\_Gateway\_RRPC\_Register|Registers the device's RRPC callback and receives RRPC requests from IoT Platform.|
|10|IOT\_Gateway\_RRPC\_Response|Responds to the RRPC requests from IoT Platform.|
|11|IOT\_Gateway\_Generate\_Message\_ID|Generates a message ID.|
|12|IOT\_Gateway\_Get\_TOPO|Sends packets to topo/get topic and waits for the reply from TOPIC\_GET\_REPLY.|
|13|IOT\_Gateway\_Get\_Config|Sends packets to conifg/get topic and waits for the reply from TOPIC\_CONFIG\_REPLY.|
|14|IOT\_Gateway\_Publish\_Found\_List|Reports a list of discovered devices.|
|**Linkkit \(\)**|
|1|linkkit\_start|Starts linkkit service, establishes connections with IoT Platform, and registers the callback.|
|2|linkkit\_end|Stops linkkit service, disconnects from IoT Platform, and releases resources.|
|3|linkkit\_dispatch|Event dispatch function that triggers the callback registered by linkkit\_start.|
|4|linkkit\_yield|The main cyclic function that includes the keep-alive timer and receives the packets from the server. Do not call this function if multithreading is allowed.|
|5|linkkit\_set\_value|Sets the TSL property of the object based on the identifier, if the identifier is a struct type, event output type or service output type, use a dot \('.'\) to separate these identifiers. For example, "identifier1.identifier2" specifies a certain item.|
|6|linkkit\_get\_value|Gets the TSL properties of the object based on the identifier.|
|7|linkkit\_set\_tsl|Reads TSL files locally, generates objects, and adds these objects to the linkkit.|
|8|linkkit\_answer\_service|Responds to the requests from cloud services.|
|9|linkkit\_invoke\_raw\_service|Sends raw data to IoT Platform.|
|10|linkkit\_trigger\_event|Reports device events to IoT Platform.|
|11|linkkit\_fota\_init|Initializes OTA-FOTA service and registers the callback. You need to compile the macro SERVICE\_OTA\_ENABLED first.|
|12|linkkit\_invoke\_fota\_service|Performs a FOTA update.|
|13|linkkit\_cota\_init|Initializes OTA-COTA service and registers the callback.  You need to compile the macro SERVICE\_OTA\_ENABLED SERVICE\_COTA\_ENABLED first.|
|14|linkkit\_invoke\_cota\_get\_config|Sends requests for remote configuration.|
|15|linkkit\_invoke\_cota\_service|Performs a COTA update.|
|16|linkkit\_post\_property|Reports device properties to IoT Platform.|

