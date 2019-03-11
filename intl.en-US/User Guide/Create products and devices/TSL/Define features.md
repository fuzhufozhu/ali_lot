# Define features {#task_qhm_d3j_w2b .task}

Defining features for products is to define Thing Specification Language \(TSL\), including defining properties, services, and events. This article describes how to define features in the IoT Platform console.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  In the left-side navigation pane, click **Devices** \> **Product**. 
3.  On the Products page, find the product for which you want to define features and click **View**. 
4.  Click **Define Feature**. 
5.  Add self-defined features. Click the **Add Feature** button corresponding to Self-defined Feature to add custom features for the product. You can define properties, services and events for the product. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/155227064010854_en-US.png)

    -   Define a property. In the Add Self-defined Feature dialog box, select **Properties** as the feature type. Enter information for the property and then click**OK**.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/155227064010855_en-US.png)

        The parameters of properties are listed in the following table.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Property name, for example, Power Consumption. Each feature name must be unique in the product.

 A feature name must start with a Chinese character, an English letter, or a digit, can contain Chinese characters, English letters, digits, dashes\(-\) and underscores \(\_\), and cannot exceed 30 characters in length.

 |
        |Identifier|Identifies a property. It must be unique in the product. It is the parameter identifier in Alink JSON TSL, and is used as the key when a device is reporting data of this property. Specifically, IoT Platform uses this parameter to verify and determine whether or not to receive the data. An identifier can contain English letters, digits, and underscores \(\_\), and cannot exceed 50 characters in length. For example, PowerConsumption.**Note:** An identifier cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.

|
        |Data Type|         -   int32: 32-bit integer. If you select int32, you are required to define the value range, step, and unit.
        -   float: Float. If you select float, you are required to define the value range, step, and unit.
        -   double: Double float. If you select double, you are required to define the value range, step, and unit.
        -   enum: Enumeration. You must specify enumeration items with values and descriptions. For example,1 indicates heating mode and 2 indicates cooling mode.
        -   bool: Boolean. You must specify the Boolean values. Values include 0 and 1. For example, you can use 0 to indicate disabled and 1 to indicate enabled.
        -   text: Text string. You must specify the data length. The maximum value is 2048 bytes.
        -   date: Timestamp. A UTC timestamp in string type, in milliseconds.
        -   struct: A JSON structure. Define a JSON structure, and add new JSON parameters. For example, you can define that the color of a lamp is a structure composed of three parameters: red, green, and blue. Structure nesting is not supported.
        -   array: Array. You must select a data type for the elements in the array from int32, float, double, text and struct. Make sure that the data type of elements in an array is the same and that the length of the array does not exceed 128 elements.
 **Note:** When the gateway connection protocol is Modbus, you do not set this parameter.

 |
        |Step|The smallest granularity of changes of properties, events, and input and output parameter values of services. If the data type is int32, float, or double, step is required.|
        |Unit|You can select None or a unit suitable.|
        |Read/Write Type|         -   Read/Write: GET and SET methods are supported for Read/Write requests.
        -   Read-only: Only GET is supported for Read-only requests.
 **Note:** When the gateway connection protocol is Modbus, you do not set this parameter.

 |
        |Description|Enter a description or remarks about the property. You can enter up to 100 characters.|
        |Extended Information| When the gateway connection protocol is Modbus or OPC UA, you can configure extended parameters.

         -   When the gateway connection protocol is Modbus, configure the following parameters.

            -   Operation Type:
                -   Coil Status \(read-only, 01\)
                -   Coil Status \(read and write, 01-read, 05-write\)
                -   Coil Status \(read and write, 01-read, 0F-write\)
                -   Discrete Input \(read-only, 02\)
                -   Holding Registers \(read-only, 03\)
                -   Holding Registers \(read and write, 03-read, 06-write\)
                -   Holding Registers \(read and write, 03-read, 10-write\)
                -   Input Registers \(read-only, 04\)
            -   Register Address: Enter a hexadecimal address beginning with 0x. The range is 0x0 - 0xFFFF. For example, 0xFE.
            -   Original Data Type: Multiple data types are supported, including int16, uint16, int32, uint32, int64, uint64, float, double, string, bool, and customized data \(raw data\).
            -   Switch High Byte and Low Byte in Register: Swap the first 8 bits and the last 8 bits of the 16-bit data in the register. Options:
                -   true
                -   false
            -   Switch Register Bits Sequence: Swap the bits of the original 32-bit data. Options:
                -   true
                -   false
            -   Zoom Factor: The zoom factor is set to 1 by default. It can be set to negative numbers, but cannot be set to 0.
            -   Collection Interval: The time interval of data collection. It is in milliseconds and the value cannot be lower than 10.
            -   Data Report: The trigger of data report. It can be either At Specific Time or Report Changes.
        -   When the gateway connection protocol is OPC UA, set a node name. Each node name must be unique under the property.
 |

    -   Define a service. In the Add Self-defined Feature dialog box, select **Services** as the feature type. Enter information for the service and then click **OK**.

        **Note:** When the gateway connection protocol is Modbus, you cannot define any service for the product.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/155227064010856_en-US.png)

        The parameters of services are as follows.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Service name.

 A feature name must start with an English letter, Chinese character, or a number. It can contain English letters, Chinese characters, digits, dashes \(-\), and underscores \(\_\), and cannot exceed 30 characters in length.

 If you have selected a category with feature template when you were creating the product, the system displays the standard services from the standard feature library for you to choose.

 **Note:** When the gateway connection protocol is Modbus, you cannot define custom services for the product.

 |
        |Identifier|Identifies a service. It must be unique within the product. The parameter identifier in Alink JSON TSL. It is used as the key when this service is called. An identifier can contain English letters, digits, and underscores \(\_\), and cannot exceed 30 characters in length.**Note:** Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value.

|
        |Invoke Method|         -   Asynchronous: For an asynchronous call, IoT Platform returns the result directly after the request is sent, and does not wait for a response from the device.
        -   Synchronous: For a synchronous call, IoT Platform waits for a response from the device. If no response is received, the call times out.
 |
        |Input Parameters|\(Optional\) Set input parameters for the service.Click **Add Parameter**, and add an input parameter in the dialog box that appears.

When the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value.
        -   You can either use a property as an input parameter or define an input parameter. For example, you can specify the properties Sprinkling Interval and Sprinkling Amount as the input parameters of the Automatic Sprinkler service feature. Then, when Automatic Sprinkler is called, the sprinkler automatically starts irrigation according to the sprinkling interval and amount.
        -   You can add up to 20 input parameters for a service.
|
        |Output Parameters|\(Optional\) Set output parameters for the service.Click **Add Parameter**, and add an output parameter in the dialog box that appears.

When the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value.
        -   You can either use a property as an output parameter or define an output parameter. For example, you can specify the property SoilHumidity as an output parameter. Then, when the service Automatic Sprinkler is called, IoT Platform returns the data about soil humidity.
        -   You can add up to 20 output parameters for a service.
|
        |Extended Information|When the gateway connection protocol is OPC UA, set a node name. Each node name must be unique under the service.|
        |Description|Enter a description or remarks about the service. You can enter up to 100 characters.|

    -   Define an event. In the Add Self-defined Feature dialog box, select **Events** as the feature type. Enter information for the parameter and then click **OK**.

        **Note:** When the gateway connection protocol is Modbus, you cannot define any event for the product.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/155227064110857_en-US.png)

        The parameters of events are as follows.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Event name.

 A feature name must start with a Chinese character, an English letter, or a digit, can contain Chinese characters, English letters, digits, dashes\(-\) and underscores \(\_\), and cannot exceed 30 characters in length.

 **Note:** When the gateway connection protocol is Modbus, you cannot define events.

 |
        |Identifier|Identifies an event. It must be unique in the product. It is the parameter identifier in Alink JSON TSL, and is used as the key when a device is reporting data of this event, for example, ErrorCode.**Note:** Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value.

|
        |Event Type|         -   Info: Indicates general notifications reported by devices, such as the completion of a specific task.
        -   Alert: Indicates alerts that are reported by devices when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
        -   Error: Indicates errors that are reported by devices when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
 |
        |Output Parameters|The output parameters of an event. Click **Add Parameter**, and add an output parameter in the dialog box that appears. You can either use a property as an output parameter or define an output parameter. For example, you can specify the property Voltage as an output parameter. Then, devices report errors with the current voltage value for further fault diagnosis.When the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value.
        -   You can add up to 20 output parameters for an event.
|
        |Extended Information|When the gateway collection protocol is OPC UA, set a node name. Each node name must be unique under the event.|
        |Description|Enter a description or remarks about the event. You can enter up to 100 characters.|


