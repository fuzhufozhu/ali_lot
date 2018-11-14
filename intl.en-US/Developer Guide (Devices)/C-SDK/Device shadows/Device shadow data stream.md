# Device shadow data stream {#concept_f1f_v1v_wdb .concept}

IoT Platform predefines two topics for each device to enable data transmission. The predefined topics have fixed formats.

-   Topic: /shadow/update/$\{YourProductKey\}/$\{YourDeviceName\}

    Devices and applications publish messages to this topic. When IoT Platform receives messages from this topic, it will extract the status information in the messages and will update the status to the device shadow.

-   Topic: /shadow/get/$\{YourProductKey\}/$\{YourDeviceName\}

    The device shadow updates the status to this topic, and the device subscribes to the messages from this topic.


Take a lightbulb device of a product bulb\_1 as an example to introduce the communication among devices, device shadows, and applications. In the following example, the ProductKey is 10000 and the DeviceName is lightbulb. The device publishes messages to and subscribes to messages of the two custom topics using the method of QoS 1.

## Device reports status automatically {#section_ojj_4nx_wdb .section}

The flow chart is shown in [Figure 1](#fig_bc3_m4x_wdb).

![](images/3219_en-US.PNG "Device reports status automatically")

1.  When the lightbulb is online, the device uses topic `/shadow/update/10000/lightbulb` to report the latest status to the device shadow.

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
    |method|The operation type when a device or application requests the device shadow.When you update the status, This parameter method is required and must be set to update.

|
    |state|The status information that the device sends to the device shadow.The reported field is required. The status information is synchronized to the reported field of the device shadow.

|
    |version|The version information contained in the request.The device shadow only accepts the request and updates to the specified version when the new version is later than the current version.

|

2.  When the device shadow accepts the status reported by the device lightbulb, the JSON file of device shadow is successfully updated.

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

3.  After the device shadow has been updated, it will return the result to the device \(lightbulb\) by sending a message to the topic `/shadow/get/10000/lightbulb`.

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

    Error codes are described in [Table 2](#table_bj2_pqx_wdb).

    |errorCode|errorMessage|
    |:--------|:-----------|
    |400|Incorrect JSON file.|
    |401|The method field is not found.|
    |402|the state field is not found.|
    |403|Invalid version field.|
    |404|The reported field is not found.|
    |405|The reported field is empty.|
    |406|Invalid method field.|
    |407|The JSON file is empty.|
    |408|The reported field contains more than 128 attributes.|
    |409|Version conflict.|
    |500|Server exception.|


## Application changes device status {#section_ql1_zqx_wdb .section}

The flow chart is shown in [Figure 2](#fig_hvt_trx_wdb).

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

3.  After the update, the device shadow sends a message to the topic `/shadow/get/10000/lightbulb` and returns the result of update to the device. The result message is created by the device shadow.

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

4.  When the device lightbulb is online and has subscribed to the topic `/shadow/get/10000/lightbulb`, the device receives the message and changes its color to green according to the desired field in the request file. After the device has updated the status, it will report the latest status to the cloud.

    ```
    {
    method": "update", 
    "state": { 
    "reported": { 
    "color": "green" 
    } 
    }, 
    "version": 3 
    }
    ```

    If the timestamp shows that the command has expired, you give up the update.

5.  After the latest status has been reported successfully, the device client sends a message to the topic `/shadow/update/10000/lightbulb` to empty the property of desired field. The message is as follows:

    ```
    
    {
    "method": "update",
    "state": {
    "desired":"null"
    },
    "version": 4
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
    "version" : 4
    }
    ```


## Devices request for device shadows {#section_ksz_51y_wdb .section}

The flow chart is shown in [Figure 3](#fig_vps_dcy_wdb).

![](images/3272_en-US.png "The device requests for device shadow")

1.  The device lightbulb sends a message to the topic `/shadow/update/10000/lightbulb` and obtains the latest status saved in the device shadow. The message is as follows:

    ```
    
    {
    "method": "get"
    }
    ```

2.  When the device shadow receives above message, the device shadow sends a message to the topic `/shadow/get/10000/lightbulb`. The message is as follows:

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


## Devices delete device shadow attributes {#section_fkh_3dd_xdb .section}

The flow chart is shown in [Figure 4](#fig_pc3_xdd_xdb).

![](images/3353_en-US.PNG "Delete device shadow attributes")

The device lightbulb is to delete the specified attributes saved in the device shadow. The device sends a JSON message to the topic `/shadow/update/10000/lightbulb`. See the message in the following example.

To delete attributes, set the value of method to delete and set the values of the attributes to null.

-   Delete one attribute:

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

-   Delete all the attributes:

    ```
    
    {
    "method": "delete",
    "state": {
    "reported":"null"
    },
    "version": 1
    }
    ```


