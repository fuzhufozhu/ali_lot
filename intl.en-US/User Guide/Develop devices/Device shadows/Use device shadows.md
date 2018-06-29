# Use device shadows {#task_us1_qk4_12b .task}

This topic describes the communication between devices, device shadows, and applications.

A device shadow is the shadow that is built on IoT Platform based on a special topic for the related device. This device synchronizes status to the cloud using this device shadow. The cloud can quickly obtain the device status from the device shadow even when the device is not connected to IoT Platform.

1.   The C SDK provides the IOT\_Shadow\_Construct function to create the device shadow. 

    The function is declared as follows:

    ```
    /**
    * @brief Construct the device shadow.
    * This function is used to initialize the data structures, establish MQTT-based connections.
    * and subscribe to the topic: "/shadow/get/${product_key}/${device_name}".
    *
    * @param [in] pparam: The specific initial parameter.
    * @retval NULL : The construction of the shadow failed.
    * @retval NOT_NULL : The construction is successful.
    * @see None.
    */
    void *IOT_Shadow_Construct(iotx_shadow_para_pt pparam);
    ```

2.   Use the IOT\_Shadow\_RegisterAttribute function to register the properties of the device shadow. 

    The function is declared as follows:

    ```
    /**
    * @brief Create a data type registered to the server.
    *
    * @param [in] handle: The handle of the device shadow.
    * @param [in] pattr: The parameter registered to the server.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_RegisterAttribute(void *handle, iotx_shadow_attr_pt pattr);
    ```

3.   You can use the IOT\_Shadow\_Pull function in the C SDK to synchronize device status to IoT Platform whenever the device shadow starts. 

    The function is declared as follows:

    ```
    
    /**
    * @brief Synchronize device shadow data to the cloud.
    * It is a synchronization function.
    * @param [in] handle: The handle of the device shadow.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_Pull(void *handle);
    ```

4.   When the device updates its status, you can use IOT\_Shadow\_PushFormat\_Init, IOT\_Shadow\_PushFormat\_Add, and IOT\_Shadow\_PushFormat\_Finalize in the C SDK to update the device status, and use IOT\_Shadow\_Push in the C SDK to synchronize the status to the cloud. 

    The function is declared as follows:

    ```
    
    
    /**
    * @brief Start processing the structure of the data type format.
    *
    * @param [in] handle: The handle of the device shadow.
    * @param [out] pformat: The format structure of the device shadow.
    * @param [in] buf: The buffer that stores the device shadow.
    * @param [in] size: The maximum length of the device shadow attribute.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_PushFormat_Init(
                        void *handle,
                        format_data_pt pformat,
                        char *buf,
                        uint16_t size);
    
    
    /**
    * @brief The format of the attribute name and value for the update.
    *
    * @param [in] handle: The handle of the device shadow.
    * @param [in] pformat: The format structure of the device shadow.
    * @param [in] pattr: The data type format created in the added member attributes.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_PushFormat_Add(
                        void *handle,
                        format_data_pt pformat,
                        iotx_shadow_attr_pt pattr);
    
    
    /**
    * @ Brief Complete processing the structure of the data type format.
    *
    * @param [in] handle: The handle of the device shadow.
    * @ Param [in] pformat: The format structure of the device shadow.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_PushFormat_Finalize(void *handle, format_data_pt pformat);
    ```

5.   To disconnect the device from IoT Platform, use IOT\_Shadow\_DeleteAttribute and IOT\_Shadow\_Destroy in the C SDK to delete all properties that have been created for this device on IoT Platform, and release the device shadow. 

    The function is declared as follows:

    ```
    
    /**
    * @brief Deconstruct the specific device shadow.
    *
    * @param [in] handle: The handle of the device shadow.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_Destroy(void *handle);
    ```


