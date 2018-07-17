# Connect sub-devices to IoT Platform {#task_r4s_yh5_vdb .task}

[Gateways and sub-devices](reseller.en-US/User Guide/Create products and devices/Gateways and sub-devices.md#)Each device, either a gateway or a sub-device, works as a unique device on IoT Platform. Devices can use unique certificates for authentication when communicating with the cloud. You need to install the unique certificates to each device, including ProductKey, DeviceName, and DeviceSecret. Some sub-devices, such as Bluetooth devices and Zigbee devices, have high requirements for installing these unique certificates. You can select dynamic registration for authentication. In this way, you only need to register sub-devices in the cloud by providing ProductKey and DeviceName.

The gateway has connected to the cloud by using [Unique-certificate-per-device authentication](reseller.en-US/User Guide/Develop devices/Authenticate devices /Unique-certificate-per-device authentication.md#).

The ProductKey and DeviceName of the sub-device must be provided on IoT Platform before dynamic registration. When a gateway registers its sub-device, IoT Platform verifies DeviceName of this sub-device. After the DeviceName is verified, IoT Platform issues the DeviceSecret.

Follow these steps:

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot) . 
2.   Configure the gateway SDK. 

    **Note:** A gateway can register its sub-devices, bring its sub-devices online or offline, maintain the topological relationship between the gateway and its sub-devices, and relay the communication between the sub-devices and IoT Platform. The manufacturer of the gateway device develops application features based on this SDK, such as connecting sub-devices to IoT Platform, receiving messages from sub-devices, publishing messages to sub-device topics to report status, subscribing to sub-device topics to obtain commands from IoT Platform, and routing messages to sub-devices.

    1.  Download the SDK. For more information, see [Download device SDKs](https://partners-intl.aliyun.com/help/doc-detail/42648.htm). This section takes a C SDK for example. 
    2.   Log on to the Linux virtual machine \(VM\) and configure unique certificates of the gateway. 
    3.   Enable the feature of the gateway and sub-devices in this SDK. 

        You can configure this SDK by using code in `iotx-sdk-c\src\subdev` and following the demo that is provided in `sample\subdev`.

        The example code consists of the following parts:

        -   Use the function in subdev to configure this SDK.

            ```
            demo_gateway_function(msg_buf, msg_readbuf);
            ```

        -   Examples of using the functions provided in the subdev\_example\_api.h file \(encapsulation of topics\) to develop code for gateways.

            ```
            demo_thing_function(msg_buf, msg_readbuf);
            ```

        -   Examples of using the functions provided in the subdev\_example\_api.h file \(encapsulation of topics\) to develop code for devices.

            ```
            demo_only_one_device(msg_buf, msg_readbuf);
            ```

        Add sub-devices to a gateway:

        -   To enable unique-certificate-per-device authentication, register your sub-devices in IoT Platform and the gateway must have the ProductKey, DeviceName, and DeviceSecret values of the sub-devices. The gateway then uses IOT\_Thing\_Register/IOT\_Subdevice\_Register to register the sub-devices \(registered type: IOTX\_Thing\_REGISTER\_TYPE\_STATIC\).
        -   To enable dynamic authentication, you need to register sub-devices on IoT Platform. The gateway uses IOT\_Thing\_Register/IOT\_Subdevice\_Register to dynamically register the sub-devices \(registered type: IOTX\_Thing\_REGISTER\_TYPE\_DYNAMIC\).

            For more information about dynamic registration, see demo\_gateway\_function.

        -   The functions provided in the example/subdev\_example\_api.c/.h file encapsulate topics for properties, events, and services. You can use these functions directly without working with the corresponding topics.
        -   To specify the features of the gateway and sub-devices, you need to define `FEATURE_SUBDEVICE_ENABLED = y` in make.settings.

            To specify the features of the gateway or a sub-device, you need to define `FEATURE_SUBDEVICE_STATUS = subdevice` in make.settings.

3.   The gateway establishes Message Queuing Telemetry Transport \(MQTT\) connections with IoT Platform. 
4.   Register a sub-device. 

    The gateway obtains the ProductKey and DeviceName of the sub-device, and uses dynamic registration to obtain DeviceSecret from IoT Platform. We recommend that you use a global unique identifier \(GUID\), such as the medium access control \(MAC\) address, as the DeviceDame.

    -   Request topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/sub/register

        Request format:

        ```
        {"id" : 123,"version":"1.0","params" : [{ "deviceName" : "deviceName1234", "productKey" : "1234556554"}],"method":"thing.sub.register"}
        ```

    -   Response topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/sub/register\_reply

        Response format:

        ```
        {"id":123,"code":200,"data":[{ "iotId":"12344", "productKey":"xxx", "deviceName":"xxx", "deviceSecret":"xxx"}]}
        ```

    -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName values of the gateway.
    -   The gateway and the sub-device send messages based on Quality of Service 0 \(QoS 0\).
    -   The corresponding function is IOT\_Subdevice\_Register in this SDK. In this function, register\_type supports static registration and dynamic registration. For more information about how to use this function and signing computing, see sample\\subdev\\subdev-example.c.
    -   If you set register\_type to IOTX\_SUBDEV\_REGISTER\_TYPE\_DYNAMIC, you can see an offline sub-device added to the gateway in the console after you use this function.
    -   If you set register\_type to IOTX\_SUBDEV\_REGISTER\_TYPE\_DYNAMIC, you only need to call this function once. If you call this function again, IoT Platform reports that the device already exists.
    -   The current version of the SDK has a limitation. The system saves the device\_secret value generated during dynamic registration in a global variable of the device. Therefore, the device\_secret value is not persistent. The DeviceSecret is lost after you restart this device.

        If you need to use this function, modify iotx\_subdevice\_parse\_register\_reply in the iotx-sdk-c\\src\\subdev\\iotx\_subdev\_api.c file to write device\_secret to a module that supports data persistence.

    -   If you set register\_type to IOTX\_SUBDEV\_REGISTER\_TYPE\_STATIC, after you use this function, you can see an existing sub-device in the console added to the gateway as an offline sub-device.

        ```
        
        /**
        * @brief Device register
        * This function is used to register a device and add topology.
        *
        * @param handle pointer to specify the gateway construction.
        * @param register type.
        * IOTX_SUBDEV_REGISTER_TYPE_STATIC
        * IOTX_SUBDEV_REGISTER_TYPE_DYNAMIC
        * @param product key.
        * @param device name.
        * @param timestamp. [if type = dynamic, must be NULL ]
        * @param client_id. [if type = dynamic, must be NULL ]
        * @param sign. [if type = dynamic, must be NULL ]
        * @param sign_method. 
        * IOTX_SUBDEV_SIGN_METHOD_TYPE_SHA
        * IOTX_SUBDEV_SIGN_METHOD_TYPE_MD5
        *
        * @return 0, Logout success; -1, Logout fail.
        */
        int IOT_Subdevice_Register(void* handle, 
        iotx_subdev_register_types_t type, 
        const char* product_key, 
        const char* device_name,
        Char * timestamp, 
        char* client_id, 
        char* sign,
        iotx_subdev_sign_method_types_t sign_type);
        ```

5.   Build the topological relationship between the gateway and the sub-device in the cloud. 
    -   Add a topological relationship.

        -   Request topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/add

            Request format:

            ```
            
            {
            "id" : "123",
            "version":"1.0",
            "params" : [{
            "deviceName" : "deviceName1234",
            "productKey" : "1234556554",
            "sign":"",
            "signmethod":"hmacSha1" //Supports hmacSha1, hmacSha256, and hmacMd5.
            "timestamp":"xxxx",
            "clientId":"xxx",//Indicates a local identifier, and can be identical with productKey&deviceName.
            }],
            "method":"thing.topo.add"
            }
            ```

        -   Response topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/add\_reply

            Request format:

            ```
            
            {
            "id":"123",
            "code":200,
            "data":{}
            }
            ```

        -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName values of the gateway.
        -   The gateway and the sub-device send messages based on QoS 0.
        -   The IOT\_Subdevice\_Register function has encapsulated this feature of the SDK. You do not need to use other functions.
    -   Delete the topological relationship.

        -   Request topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/delete

            Request format:

            ```
            
            {
            "id" : 123,
            "version":"1.0",
            "params" : [{
            "deviceName" : "deviceName1234",
            "productKey" : "1234556554"
            }],
            "method":"thing.topo.delete"
            }
            ```

        -   Response topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/delete\_reply

            ```
            
            {
            "id":123,
            "code":200,
            "data":{}
            }
            ```

        -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName of the gateway.
        -   The gateway and the sub-device send messages based on QoS 0.
        -   The IOT\_Subdevice\_Unregister function has encapsulated this feature of the SDK. You do not need to use other functions.
    -   Obtain the topological relationship.

        -   Request topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/get

            Request format:

            ```
            
            {
            "id" : 123,
            "version":"1.0",
            "params" : {},
            "method":"thing.topo.get"
            }
            ```

        -   Response topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/topo/get\_reply

            ```
            
            {
            "id":123,
            "code":200,
            "data": [{
            "deviceName" : "deviceName1234",
            "productKey" : "1234556554"
            }]
            }
            ```

        -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName of the gateway.
        -   The gateway and the sub-device send messages based on QoS 0.
        -   The corresponding function is IOT\_Gateway\_Get\_TOPO in this SDK.
        -   You can obtain the information about all sub-devices of this gateway, including their ProductKey, DeviceName, and DeviceSecret certificates, by using this function. Response parameter get\_topo\_reply is in JSON format. You need to parse the response.

            ```
            
            /**
            * @brief Gateway get topo
            * This function is used to publish a packet with topo/get topic, and wait for the reply (with TOPO_GET_REPLY topic).
            *
            * @param handle pointer to specify the Gateway.
            * @param get_topo_reply.
            * @param length [in/out]. in -- get_topo_reply buffer length, out -- reply length
            *
            * @return 0, logout success; -1, logout failed.
            */
            int IOT_Gateway_Get_TOPO(void* handle, 
            char* get_topo_reply, 
            uint32_t* length);
            ```

6.   Connect the sub-device to IoT Platform. 

    -   Request topic: /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/login

        Request format:

        ```
        
        {
        "id":"123",
        "params":{
        "productKey":"xxxxx",// Sub-device ProductKey
        "deviceName":"xxxx",//Sub-device DeviceName
        "clientId":"xxxx",
        "timestamp":"xxxx",
        "signMethod":"hmacmd5 or hmacsha1 or hmacsha256",
        "sign":"xxxxx", //Sub-device signature
        "cleanSession":"true or false" // If this parameter is set to true, the system clears all QoS 1- or 2-based messages missed by the offline sub-device.
        }
        }
        //Sub-devices with ProductKey, DeviceName and DeviceSecret follow the same signature rules as the gateway.
        sign = hmac_md5(deviceSecret, clientId123deviceNametestproductKey123timestamp123)
        ```

    -   Response topic: /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/login\_reply

        Response format:

        ```
        
        {
        "id":"123",
        "code":200,
        "message":"success"
        }
        ```

    -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName of the gateway.
    -   The gateway and the sub-device send messages based on QoS 0.
    -   The corresponding function is IOT\_Subdevice\_Login in this SDK. For more information about how to use this function, see sample\\subdev\\subdev-example.c.
    -   You can see the sub-device in online status in the console after you call this function.
    ```
    
    /**
    * @brief Subdevice login
    * This function is used to publish a packet with the LOGIN topic, wait for the reply (with
    * LOGIN_REPLY topic), and then subscribe to some subdevice topics.
    *
    * @param handle pointer to specify the Gateway.
    * @param product key.
    * @param device name.
    * @ Param timestamp. [If aster_type = dynamic, must be null]
    * @param client_id. [if register_type = dynamic, must be NULL ]
    * @param sign. [if register_type = dynamic, must be NULL ] 
    * @param sign method, HmacSha1 or HmacMd5. 
    * @param clean session, true or false.
    *
    * @return 0, login success; -1, login failed.
    */
    int IOT_Subdevice_Login(void* handle,
    const char* product_key, 
    const char* device_name, 
    const char* timestamp, 
    const char* client_id, 
    const char* sign, 
    iotx_subdev_sign_method_types_t sign_method_type,
    iotx_subdev_clean_session_types_t clean_session_type);
    ```

7.   Interact with the sub-device. 

    -   Request topic: You can use the topic in the format of /$\{productKey\}/$\{deviceName\}/xxx specified on IoT Platform, or in the format of /sys/$\{productKey\}/$\{deviceName\}/thing/xxx.
    -   The gateway publishes data to the topic of the sub-device. In the topic, /$\{productKey\}/$\{deviceName\}/ corresponds to ProductKey and DeviceName of the sub-device.
    -   The format of the MQTT payload is unrestricted.
    -   This SDK provides three functions: IOT\_Gateway\_Subscribe, IOT\_Gateway\_Unsubscribe, and IOT\_Gateway\_Publish, to subscribe to and publish messages. For more information about how to use this function, see sample\\subdev\\subdev-example.c.
    ```
    
    /**
    * @brief Gateway Subscribe
    * This function is used to subscribe to some topics.
    *
    * @param handle pointer to specify the Gateway.
    * @param topic list.
    * @param QoS.
    * @param function to receive data.
    * @param topic_handle_func's userdata.
    *
    * @return 0, Subscribe success; -1, Subscribe fail.
    */
    int IOT_Gateway_Subscribe(void* handle, 
    const char *topic_filter, 
    int qos, 
    iotx_subdev_event_handle_func_fpt topic_handle_func, 
    void *pcontext);
    /**
    * @brief Gateway Unsubscribe
    * This function is used to unsubscribe from some topics.
    *
    * @param handle pointer to specify the Gateway.
    * @param topic list.
    *
    * @return 0, Unsubscribe success; -1, Unsubscribe fail.
    */
    int IOT_Gateway_Unsubscribe(void* handle, const char* topic_filter);
    /**
    * @brief Gateway Publish
    * This function is used to publish some packets.
    *
    * @param handle pointer to specify the Gateway.
    * @param topic.
    * @param mqtt packet.
    *
    * @return 0, Publish success; -1, Publish fail.
    */
    int IOT_Gateway_Publish(void* handle,
    const char *topic_name, 
    iotx_mqtt_topic_info_pt topic_msg);
    ```

8.   Disconnect the sub-device from IoT Platform. 

    -   Request topic: /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/logout

        Request format:

        ```
        
        {
        "id":123,
        "params":{
        "productKey":"xxxxx",//ProductKey of the sub-device
        "deviceName":"xxxxx",//DeviceName of the sub-device
        }
        }
        ```

    -   Response topic: /ext/session/\{gw\_productKey\}/\{gw\_deviceName\}/combine/logout\_reply

        ```
        
        {
        "id":"123",
        "code":200,
        "message":"success"
        }
        ```

    -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName values of the gateway.
    -   The gateway and the sub-device send messages based on QoS 0.
    -   The corresponding function is IOT\_Subdevice\_Logout in this SDK. For more information about how to use this function, see sample\\subdev\\subdev-example.c.
    -   You can see the sub-device in offline status in the console after you use this function.
    ```
    
    /**
    * @brief Subdevice logout
    * This function is used to unsubscribe from some subdevice topics, publish a packet with the
    * LOGOUT topic, and then wait for the reply (with LOGOUT_REPLY topic).
    *
    * @param handle pointer to specify the Gateway.
    * @param product key.
    * @param device name.
    *
    * @return 0, logout success; -1, logout failed.
    */
    int IOT_Subdevice_Logout(void* handle,
    const char* product_key,
    const char* device_name);
    ```

9.   Unregister the sub-device dynamically. 

    -   Request topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/sub/unregister

        Request format:

        ```
        
        {
        "id" : 123,
        "version":"1.0",
        "params" : [{
        "deviceName" : "deviceName1234",
        "productKey" : "1234556554"
        }],
        "method":"thing.sub.unregister"
        }
        ```

    -   Response topic: /sys/\{gw\_productKey\}/\{gw\_deviceName\}/thing/sub/unregister\_reply

        ```
        
        {
        "id":123,
        "code":200,
        "data":{}
        }
        ```

    -   The ProductKey and DeviceName values in the JSON object cannot be the same as the ProductKey and DeviceName of the gateway.
    -   The gateway and the sub-device send messages based on QoS 0.
    -   The corresponding function is IOT\_Subdevice\_Unregister in this SDK. For more information about how to use this function, see sample\\subdev\\subdev-example.c.
    -   The sub-device is destroyed and cannot be used after you call this function. Do not call this function if you still need the sub-device.
    ```
    
    /**
    * @brief Device unregister
    * This function is used to delete the topological relationship and unregister the device.
    * The device must dynamically register again if you want to use this device after unregistration.
    *
    * @param handle pointer to specify the gateway construction.
    * @param product key.
    * @param device name.
    *
    * @ Return 0, unregister success;-1, unregister fail.
    */
    int IOT_Subdevice_Unregister(void* handle, 
    const char* product_key, 
    const char* device_name);
    ```

    **Note:** 

    -   gw\_productKey indicates ProductKey of the gateway.
    -   gw\_deviceName indicates DeviceName of the gateway.
    For more information about other functions in this SDK, use the subdev-example code.


