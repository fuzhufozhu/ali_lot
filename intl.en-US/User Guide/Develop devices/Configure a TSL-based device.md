# Configure a TSL-based device {#concept_gdq_sv5_wdb .concept}

This topic describes how to configure a device based on a TSL model.

**Note:** Only IoT Platform Pro supports this feature.

## Prerequisites {#section_ovw_5v5_wdb .section}

Create a product, add a device, and define the TSL in the IoT Platform console. A TSL model describes the properties, services, and events of a device, as shown in figure [Figure 1](#fig_g5q_wv5_wdb).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7509/3115_en-US.png "Create devices")

## Establish a connection to IoT Platform {#section_usr_cw5_wdb .section}

1.  For more information about establishing an MQTT connection to connect a device and IoT Platform, see [Establish MQTT over TCP connections](intl.en-US/User Guide/Develop devices/Protocols for connecting devices/Establish MQTT over TCP connections.md#).
2.  Call the linkkit\_start operation in the device SDK to establish a connection to IoT Platform and subscribe to topics.

    When you use the device SDK, save a shadow for the device. A shadow is an abstraction of a device, which is used to retrieve the status information of the device. The interaction process between a device and IoT Platform is a synchronization process between the device and shadow and between the shadow and IoT Platform.

    Variable get\_tsl\_from\_cloud is used to synchronize the TSL model from IoT Platform when the device comes online.

    -   get\_tsl\_from\_cloud = 0: Indicates that a TSL model has been pre-defined. TSL\_STRING is used as the standard TSL model.

        The SDK copies the TSL model that is created in the console, uses the TSL model to define TSL\_STRING in linkkit\_sample.c, and then calls the linkkit\_set\_tsl operation to set the pre-defined TSL model.

        **Note:** Use the C escape character correctly.

    -   get\_tsl\_from\_cloud = 1: Indicates that no TSL model has been pre-defined. The SDK must dynamically retrieve the TSL model from IoT Platform.

        Dynamically retrieving a TSL model consumes a large amount of memory and bandwidth. The specific consumption depends on the complexity of the TSL model. A TSL model of 10 KB consumes about 20 KB of memory and 10 KB of bandwidth.

3.  Use the linkkit\_ops\_t parameter to register the callback.

    ```
    
    linkkit_start(8, get_tsl_from_cloud, linkkit_loglevel_debug, &alinkops, linkkit_cloud_domain_sh, sample_ctx);
    if (! get_tsl_from_cloud) {
       linkkit_set_tsl(TSL_STRING, strlen(TSL_STRING));
    }
    ```

    Function implementation:

    ```
    
    typedef struct _linkkit_ops {
      int (*on_connect)(void *ctx);
      int (*on_disconnect)(void *ctx);
      int (*raw_data_arrived)(void *thing_id, void *data, int len, void *ctx);
      int (*thing_create)(void *thing_id, void *ctx);
      int (*thing_enable)(void *thing_id, void *ctx);
      int (*thing_disable)(void *thing_id, void *ctx);
    #ifdef RRPC_ENABLED
      int (*thing_call_service)(void *thing_id, char *service, int request_id, int rrpc, void *ctx);
    #else
      int (*thing_call_service)(void *thing_id, char *service, int request_id, void *ctx);
    #endif /* RRPC_ENABLED */
      int (*thing_prop_changed)(void *thing_id, char *property, void *ctx);
    } linkkit_ops_t;
    /**
    * @brief start linkkit routines, and install callback funstions(async type for cloud connecting).
    *
    * @param max_buffered_msg, specify max buffered message size.
    * @param ops, callback function struct to be installed.
    * @param get_tsl_from_cloud, config if device need to get tsl from cloud(! 0) or local(0), if local selected, must invoke linkkit_set_tsl to tell tsl to dm after start complete.
    * @param log_level, config log level.
    * @param user_context, user context pointer.
    * @param domain_type, specify the could server domain.
    *
    * @return int, 0 when success, -1 when fail.
    */
    int linkkit_start(int max_buffered_msg, int get_tsl_from_cloud, linkkit_loglevel_t log_level, linkkit_ops_t *ops, linkkit_cloud_domain_type_t domain_type, void *user_context);
    /**
    * @brief install user tsl.
    *
    * @param tsl, tsl string that contains json description for thing object.
    * @param tsl_len, tsl string length.
    *
    * @return pointer to thing object, NULL when fails.
    */
    extern void* linkkit_set_tsl(const char* tsl, int tsl_len);
    ```

4.  After you have connected the device to IoT Platform, log on to the IoT Platform console and verify whether the device has come online.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7509/3116_en-US.png "Device comes online")


## Send property changes to IoT Platform {#section_lbl_nw5_wdb .section}

1.  When the properties of a device change, the device automatically sends the changes to IoT Platform by publishing to topic `/sys/{productKey}/{deviceName}/thing/event/property/post`.

    Request:

    ```
    
    TOPIC: /sys/{productKey}/{deviceName}/thing/event/property/post 
    REPLY TOPIC: /sys/{productKey}/{deviceName}/thing/event/property/post_reply
    request
    {
    "id" : "123",
    "version":"1.0",
    "params" : {
    "PowerSwitch" : 1
    },
    "method":"thing.event.property.post" 
    } 
    response 
    {
    "id":"123",
    "code":200,
    "data":{}
    }
    ```

2.  The SDK calls the linkkit\_set\_value operation to modify the property of the shadow, and then calls the linkkit\_trigger\_event operation to synchronize the shadow to IoT Platform.

**Note:** The device will automatically send the current property of the shadow to IoT Platform.

    Function:

    ```
    
    linkkit_set_value(linkkit_method_set_property_value, sample->thing, EVENT_PROPERTY_POST_IDENTIFIER, value, value_str); // set value
    return linkkit_trigger_event(sample->thing, EVENT_PROPERTY_POST_IDENTIFIER, NULL); // update value to cloud
    ```

    Function implementation:

    ```
    
    /**
    * @brief set value to property, event output, service output items.
    * if identifier is struct type or service output type or event output type, use '.' as delimeter like "identifier1.ientifier2"
    * to point to specific item.
    * value and value_str could not be NULL at the same time;
    * if value and value_str both as not NULL, value shall be used and value_str will be ignored.
    * if value is NULL, value_str not NULL, value_str will be used.
    * in brief, value will be used if not NULL, value_str will be used only if value is NULL.
    *
    * @param method_set, specify set value type.
    * @param thing_id, pointer to thing object, specify which thing to set.
    * @param identifier, property, event output, service output identifier.
    * @param value, value to set.(input int* if target value is int type or enum or bool, float* if float type,
    * long long* if date type, char* if text type).
    * @param value_str, value to set in string format if value is null.
    *
    * @return 0 when success, -1 when fail.
    */
    extern int linkkit_set_value(linkkit_method_set_t method_set, const void* thing_id, const char* identifier,
    const void* value, const char* value_str);
    /**
    * @brief trigger a event to post to cloud.
    *
    * @param thing_id, pointer to thing object.
    * @param event_identifier, event identifier to trigger.
    * @param property_identifier, used when trigger event with method "event.property.post", if set, post specified property, if NULL, post all.
    *
    * @return 0 when success, -1 when fail.
    */
    extern int linkkit_trigger_event(const void* thing_id, const char* event_identifier, const char* property_identifier);
    ```


## Get a device property on IoT Platform {#section_sxl_yw5_wdb .section}

1.  You can log on to the IoT Platform console and use topic `/sys/{productKey}/{deviceName}/thing/service/property/get` to get a property of a device.

    Request:

    ```
    
    TOPIC: /sys/{productKey}/{deviceName}/thing/service/property/get
    REPLY TOPIC: /sys/{productKey}/{deviceName}/thing/service/property/get_reply
    request 
    {
    "id" : "123",
    "version":"1.0",
    "params" : [
    "powerSwitch"
    ],
    "method":"thing.service.property.get" 
    } 
    response 
    {
    "id":"123",
    "code":200,
    "data":{
    "powerSwitch":0
    }
    }
    ```

2.  When the device receives the GET command from IoT Platform, the SDK executes the command to read the property value from the shadow and returns the value to IoT Platform.

## Set a device property on IoT Platform {#section_hg1_dx5_wdb .section}

1.  You can log on to the IoT Platform console and use topic `/sys/{productKey}/{deviceName}/thing/service/property/set` to set a property of a device client.

    Request:

    ```
    
    TOPIC: /sys/{productKey}/{deviceName}/thing/service/property/set
    REPLY TOPIC: /sys/{productKey}/{deviceName}/thing/service/property/set_reply
    payload: 
    {
    "id" : "123",
    "version":"1.0",
    "params" : {
    "PowerSwitch" : 0,
    },
    "method":"thing.service.property.set" 
    }
    response
    {
    "id":"123",
    "code":200,
    "data":{}
    }
    ```

2.  The SDK registers the thing\_prop\_changed callback function in the linkkit\_ops\_t parameter of the linkkit\_start method to respond to the request sent from IoT Platform for setting device properties.
3.  The linkkit\_get\_value parameter in the callback function is used to get the device property of the shadow, which is the same as the device property that is modified on IoT Platform.
4.  After setting the new property value, you can implement the linkkit\_answer\_service function to return the result to IoT Platform. You can choose whether to perform this task based on your business needs.

    Function implementation:

    ```
    
    static int thing_prop_changed(void* thing_id, char* property, void* ctx)
    {
    char* value_str = NULL;
    ... ...
    linkkit_get_value(linkkit_method_get_property_value, thing_id, property, NULL, &value_str);
    LINKKIT_PRINTF("#### property(%s) new value set: %s ####\n", property, value_str);
    }
    /* do user's process logical here. */
    linkkit_trigger_event(thing_id, EVENT_PROPERTY_POST_IDENTIFIER, property);
    return 0;
    }
    ```


Callback function:

```
int (*thing_prop_changed)(void *thing_id, char *property, void *ctx);
```

Function implementation:

```

/**
* @brief get value from property, event output, service input/output items.
* if identifier is struct type or service input/output type or event output type, use '.' as delimeter like "identifier1.ientifier2"
* to point to specific item.
* value and value_str could not be NULL at the same time;
* if value and value_str both as not NULL, value shall be used and value_str will be ignored.
* if value is NULL, value_str not NULL, value_str will be used.
* in brief, value will be used if not NULL, value_str will be used only if value is NULL.
* @param method_get, specify get value type.
* @param thing_id, pointer to thing object, specify which thing to get.
* @param identifier, property, event output, service input/output identifier.
* @param value, value to get(input int* if target value is int type or enum or bool, float* if float type,
* long long* if date type, char* if text type).
* @param value_str, value to get in string format. DO NOT modify this when function returns,
* user should copy to user's own buffer for further process.
* user should NOT free the memory.
*
* @return 0 when success, -1 when fail.
*/
extern int linkkit_get_value(linkkit_method_get_t method_get, const void* thing_id, const char* identifier,
void* value, char** value_str);
```

Function:

```

linkkit_set_value(linkkit_method_set_service_output_value, thing, identifier, &sample->service_custom_output_contrastratio, NULL);
linkkit_answer_service(thing, service_identifier, request_id, 200);
```

Function implementation:

```

/**
* @brief set value to property, event output, service output items.
* if identifier is struct type or service output type or event output type, use '.' as delimeter like "identifier1.ientifier2"
* to point to specific item.
* value and value_str could not be NULL at the same time;
* if value and value_str both as not NULL, value shall be used and value_str will be ignored.
* if value is NULL, value_str not NULL, value_str will be used.
* in brief, value will be used if not NULL, value_str will be used only if value is NULL.
*
* @param method_set, specify set value type.
* @param thing_id, pointer to thing object, specify which thing to set.
* @param identifier, property, event output, service output identifier.
* @param value, value to set.(input int* if target value is int type or enum or bool, float* if float type,
* long long* if date type, char* if text type).
* @param value_str, value to set in string format if value is null.
*
* @return 0 when success, -1 when fail.
*/
extern int linkkit_set_value(linkkit_method_set_t method_set, const void* thing_id, const char* identifier,
const void* value, const char* value_str);
/**
* @brief answer to a service when a service requested by cloud.
*
* @param thing_id, pointer to thing object.
* @param service_identifier, service identifier to answer, user should get this identifier from handle_dm_callback_fp_t type callback
* report that "dm_callback_type_service_requested" happened, use this function to generate response to the service sender.
* @param response_id, id value in response payload. its value is from "dm_callback_type_service_requested" type callback function.
* use the same id as the request to send response as the same communication session.
* @param code, code value in response payload. for example, 200 when service is successfully executed, 400 when not successfully executed.
* @param rrpc, specify rrpc service call or not.
*
* @return 0 when success, -1 when fail.
*/
extern int linkkit_answer_service(const void* thing_id, const char* service_identifier, int response_id, int code);
```

## IoT Platform requests a service from the device. {#section_v4p_sx5_wdb .section}

1.  IoT Platform uses topic `/sys/{productKey}/{deviceName}/thing/service/{dsl.service.identifer}` to invoke a service from the device. The service is defined in dsl.service.identifer of the standard TSL model.

    ```
    
    TOPIC: /sys/{productKey}/{deviceName}/thing/service/{dsl.service.identifer}
    REPLY TOPIC: 
    /sys/{productKey}/{deviceName}/thing/service/{dsl.service.identifer}_reply
    request 
    {
    "id" : "123",
    "version":"1.0",
    "params" : {
    "SprinkleTime" : 50,
    "SprinkleVolume" : 600
    },
    "method":"thing.service.AutoSprinkle" 
    } 
    response 
    {
    "id":"123",
    "code":200,
    "data":{}
    }
    ```

2.  The SDK registers the thing\_call\_service callback function in the linkkit\_ops\_t parameter of the linkkit\_start method, to send a response to the service request.
3.  After setting the new property value, you must call the linkkit\_answer\_service function to send a response to IoT Platform.

    Function:

    ```
    int (*thing_call_service)(void *thing_id, char *service, int request_id, void *ctx);
    ```

    Function implementation:

    ```
    
    static int handle_service_custom(sample_context_t* sample, void* thing, char* service_identifier, int request_id)
    {
    char identifier[128] = {0};
    /*
    * get iutput value.
    */
    snprintf(identifier, sizeof(identifier), "%s.%s", service_identifier, "SprinkleTime");
    linkkit_get_value(linkkit_method_get_service_input_value, thing, identifier, &sample->service_custom_input_transparency, NULL);
    /*
    * set output value according to user's process result.
    */
    snprintf(identifier, sizeof(identifier), "%s.%s", service_identifier, "SprinkleVolume");
    sample->service_custom_output_contrastratio = sample->service_custom_input_transparency >= 0 ? sample->service_custom_input_transparency : sample->service_custom_input_transparency * -1;
    linkkit_set_value(linkkit_method_set_service_output_value, thing, identifier, &sample->service_custom_output_contrastratio, NULL);
    linkkit_answer_service(thing, service_identifier, request_id, 200);
    return 0;
    }
    ```


## Send events to IoT Platform {#section_r4h_by5_wdb .section}

1.  A device subscribes to topic `/sys/{productKey}/{deviceName}/thing/event/{dsl.event.identifer}/post` to send an event to IoT Platform. The event is defined in dsl.event.identifer of the standard TSL model.

    Request:

    ```
    
    TOPIC: /sys/{productKey}/{deviceName}/thing/event/{dsl.event.identifer}/post 
    REPLY TOPIC: /sys/{productKey}/{deviceName}/thing/event/{dsl.event.identifer}/post_reply 
    request 
    {
    "id" : "123",
    "version":"1.0",
    "params" : {
    "ErrorCode" : 0
    },
    "method":"thing.event.Error.post" 
    } 
    response:
    {
    "id" : "123",
    "code":200,
    "data" : {}
    }
    ```

2.  The SDK calls the linkkit\_trigger\_event method to send an event to IoT Platform.

    Function:

    ```
    
    static int post_event_error(sample_context_t* sample)
    {
    char event_output_identifier[64];
    snprintf(event_output_identifier, sizeof(event_output_identifier), "%s.%s", EVENT_ERROR_IDENTIFIER, EVENT_ERROR_OUTPUT_INFO_IDENTIFIER);
    int errorCode = 0;
    linkkit_set_value(linkkit_method_set_event_output_value,
    sample->thing,
    event_output_identifier,
    &errorCode, NULL);
    return linkkit_trigger_event(sample->thing, EVENT_ERROR_IDENTIFIER, NULL);
    }
    ```


Function implementation:

```

/**
* @brief trigger a event to post to cloud.
*
* @param thing_id, pointer to thing object.
* @param event_identifier, event identifier to trigger.
* @param property_identifier, used when trigger event with method "event.property.post", if set, post specified property, if NULL, post all.
*
* @return 0 when success, -1 when fail.
*/
extern int linkkit_trigger_event(const void* thing_id, const char* event_identifier, const char* property_identifier);
```

