# Device shadow data stream {#concept_f1f_v1v_wdb .concept}

IoT Platform predefines two topics for each device to enable data transmission. The predefined topics have fixed formats.

-   topic/shadow/update/$\{productKey\}/$\{deviceName\}

    The device and application publish messages to this topic. IoT Platform updates the status to the device shadow after receiving messages from this topic.

-   topic/shadow/get/$\{productKey\}/$\{deviceName\}

    The device shadow updates the status to this topic, and the device subscribes to the messages from this topic.


The following section takes the lightbulb \(productkey:10000 and deviceName:lightbulb\) as an example and demonstrates the communication among the device, device shadow, and the application. The device publishes and subscribes to the predefined topics with QoS 1.

## Device reports status automatically {#section_ojj_4nx_wdb .section}

The flow chart is as shown in [Figure 1](#fig_bc3_m4x_wdb).

![](images/3219_en-US.PNG "Device reports status automatically")

1.  When the lightbulb is online, the device uses topic /shadow/update/10000/lightbulb to report the latest status to the device shadow.

    Format of the JSON message:

    ```
    
    {
    "method": "update",
    "state": {
    "reported": {
    "color": "red"
    }
    },
    "version": 1
    }
    ```

    The JSON parameters are described in [Table 1](#table_qcp_3px_wdb).

    |Parameter|Description|
    |---------|-----------|
    |method|Represents the operation type when the device or application requests the device shadow.This parameter must be set to "update" when you perform updates.

|
    |state|Represents the status information that the device sends to the device shadow.The reported field is required. The status information is synchronized to the reported field of the device shadow.

|
    |version|Represents the version information contained in the request.The device shadow only accepts the request and updates to the specified version when the new version is later than the current version.

|

2.  When the device shadow accepts the status reported by the lightbulb, the device shadow JSON file is successfully updated.

    ```
    
    {
    "state" : {
    "reported" : {
    "color" : "red"
    }
    },
    "metadata" : {
    "reported" : {
    "color" : {
    "timestamp" : 1469564492
    }
    }
    },
    "timestamp" : 1469564492
    "version" : 1
    }
    ```

3.  After the update, the device shadow sends the result of the update to the device by sending a message to topic /shadow/get/10000/lightbulb. The device is subscribed to this topic.

    -   If the update is successful, the message is as follows:

        ```
        
        {
        "method":"reply",
        "payload": {
        "status":"success",
        "version": 1
        },
        "timestamp": 1469564576
        }
        ```

    -   If an error occurred during the update, the message is as follows:

        ```
        
        {
        "method":"reply",
        "payload": {
        "status":"error",
        "content": {
        "errorcode": "${errorcode}",
        "errormessage": "${errormessage}"
        }
        },
        "timestamp": 1469564576
        }
        ```

    The meaning of the error codes is described in [Table 2](#table_bj2_pqx_wdb). 

    |errorCode|errorMessage|
    |---------|------------|
    |400|Invalid JSON format.|
    |401|The method field is missing.|
    |402|The state field is missing.|
    |403|Invalid version field.|
    |404|The reported field is missing.|
    |405|The reported field is blank.|
    |406|Invalid method field.|
    |407|The JSON file is empty.|
    |408|The reported field contains more than 128 attributes.|
    |409|Version conflict.|
    |500|Server exception.|


## Application changes device status {#section_ql1_zqx_wdb .section}

The flow chart is as shown in [Figure 2](#fig_hvt_trx_wdb).

![](images/3232_en-US.png "Application changes device status")

1.  The application sends a command to the device shadow to change the status of the lightbulb.

    The application sends a message to topic`/shadow/update/10000/lightbulb/`. The message is as follows:

    ```
    
    {
    "method": "update",
    "state": {
    "desired": {
    "color": "green"
    }
    },
    "version": 2
    }
    ```

2.  The application sends an update request to update the device shadow JSON file. The device shadow JSON file is changed to:

    ```
    
    {
    "state" : {
    "reported" : {
    "color" : "red"
    },
    "desired" : {
    "color" : "green"
    }
    },
    "metadata" : {
    "reported" : {
    "color" : {
    "timestamp" : 1469564492
    }
    },
    "desired" : {
    "color" : {
    "timestamp" : 1469564576
    }
    }
    },
    "timestamp" : 1469564576,
    "version" : 2
    }
    ```

3.  After the update, the device shadow sends a message to topic `/shadow/get/10000/lightbulb` and returns the result of update to the device. The result message is created by the device shadow. 

    ```
    
    {
    "method":"control",
    "payload": {
    "status":"success",
    "state": {
    "reported": {
    "color": "red"
    },
    "desired": {
    "color": "green"
    }
    },
    "metadata": {
    "reported": {
    "color": {
    "timestamp": 1469564492
    }
    },
    "desired" : {
    "color" : {
    "timestamp" : 1469564576
    }
    }
    }
    },
    "version": 2,
    "timestamp": 1469564576
    }
    ```

4.  When the lightbulb is online and subscribed to topic `/shadow/get/10000/lightbulb`, the lightbulb receives the message and changes its color to green based on the desired field in the request file.

    If the timestamp shows that the command has expired, you can choose to not update the lightbulb status.

5.  After the update, the device sends a message to topic `/shadow/update/10000/lightbulb` to report the latest status. The message is as follows:

    ```
    
    {
    "method": "update",
    "state": {
    "desired":"null"
    },
    "version": 3
    }
    ```

6.  After the status has been reported, the device shadow is synchronously updated. The device shadow JSON file is as follows: 

    ```
    
    {
    "state" : {
    "reported" : {
    "color" : "green"
    }
    },
    "metadata" : {
    "reported" : {
    "color" : {
    "timestamp" : 1469564577
    }
    },
    "desired" : {
    "timestamp" : 1469564576
    }
    },
    "version" : 3
    }
    ```


## Device obtains device shadow {#section_ksz_51y_wdb .section}

The flow chart is as follows:

![](images/3272_en-US.png "Device obtains device shadow")

1.  The lightbulb sends a message to topic `/shadow/update/10000/lightbulb` and obtains the latest status saved in the device shadow. The message is as follows:

    ```
    
    {
    "method": "get"
    }
    ```

2.  When the device shadow receives above message, the device shadow sends a message to topic `/shadow/get/10000/lightbulb`. The lightbulb is subscribed to this topic, and the message is as follows:

    ```
    
    {
    "method":"reply",
    "payload": {
    "status":"success",
    "state": {
    "reported": {
    "color": "red"
    },
    "desired": {
    "color": "green"
    }
    },
    "metadata": {
    "reported": {
    "color": {
    "timestamp": 1469564492
    }
    },
    "desired": {
    "color": {
    "timestamp": 1469564492
    }
    }
    }
    },
    "version": 2,
    "timestamp": 1469564576
    }
    ```

3.  The SDK uses the IOT\_Shadow\_Pull operation to obtain the device shadow.

    ```
    IOT_Shadow_Pull(h_shadow);
    ```

    Function prototype:

    ```
    
    /**
    * @brief Synchronize device shadow data from cloud.
    * It is a synchronous interface.
    * @param [in] handle: The handle of device shaodw.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_Pull(void *handle);
    ```


## Device deletes device shadow attributes {#section_fkh_3dd_xdb .section}

The flow chart is as follows:

![](images/3353_en-US.PNG "Delete device shadow attributes")

1.  The lightbulb wants to delete the specified attributes saved in the device shadow. The lightbulb sends a JSON message to topic `/shadow/update/10000/lightbulb`. The message is as follows:

    -   The JSON format for deleting the specified attributes:

        ```
        
        {
        "method": "delete",
        "state": {
        "reported": {
        "color": "null",
        "temperature":"null"
        }
        },
        "version": 1
        }
        ```

    -   The JSON format for deleting all attributes:

        ```
        
        {
        "method": "delete",
        "state": {
        "reported":"null"
        },
        "version": 1
        }
        ```

    To delete attributes, you need to set method to delete and set the values of the attributes to null.

2.  The SDK uses the IOT\_Shadow\_DeleteAttribute operation to delete device shadow attributes. The following parameters are provided:

    ```
    
    IOT_Shadow_DeleteAttribute(h_shadow, &attr_temperature);
    IOT_Shadow_DeleteAttribute(h_shadow, &attr_light);
    IOT_Shadow_Destroy(h_shadow);
    ```

    Function prototype:

    ```
    
    /**
    * @brief Delete the specific attribute.
    *
    * @param [in] handle: The handle of device shaodw.
    * @param [in] pattr: The parameter to be deleted from server.
    * @retval SUCCESS_RETURN : Success.
    * @retval other : See iotx_err_t.
    * @see None.
    */
    iotx_err_t IOT_Shadow_DeleteAttribute(void *handle, iotx_shadow_attr_pt pattr);
    ```


