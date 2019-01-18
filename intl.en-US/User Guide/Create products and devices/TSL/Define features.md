# Define features {#task_qhm_d3j_w2b .task}

This topic describes how to define features in the IoT Platform console.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  In the left-side navigation pane, click **Devices** \> **Product**. 
3.  On the Products page, find the product for which you want to define features and click **View**. 
4.  On the product details page, click **Define Feature**. 
5.  Add standard features. Click the **Add Feature** button corresponding to Standard Feature, and then in the dialog box, select the pre-defined features that can be used for your product. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154777736932042_en-US.png)

6.  Add self-defined features. Click the **Add Feature** button corresponding to Self-Defined Feature, and then define custom features for the product. You can define properties, services and events for the product. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154777736910854_en-US.png)

    -   Define a property. In the Add Self-defined Feature dialog box, select **Properties** as the feature type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154777736910855_en-US.png)

        The parameters of properties are listed in the following table.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Property name, for example, Power Consumption. Each feature name must be unique in the product.

 A feature name must start with a Chinese character, an English letter, or a digit, can contain Chinese characters, English letters, digits, dashes \(-\) and underscores \(\_\), and cannot exceed 30 characters in length.

 If you have selected a category with feature template when you were creating the product, the system displays the standard properties from the standard feature library for you to choose.

 **Note:** If the gateway connection protocol is Modbus, standard properties are not supported. Instead, you must define properties manually.

 |
        |Identifier|Identifies a property. It must be unique in the product. It is the parameter identifier in Alink JSON TSL, and is used as the key when a device is reporting data of this property. Specifically, IoT Platform uses this parameter to verify and determine whether or not to receive the data. An identifier can contain English letters, digits, and underscores \(\_\), and cannot exceed 50 characters in length. For example, PowerConsuption.**Note:** An identifier cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.

|
        |Data Type|         -   int32: 32-bit integer. If you select int32, you are required to define the value range, step, and unit.
        -   float: Float. If you select float, you are required to define the value range, step, and unit.
        -   double: Double float. If you select double, you are required to define the value range, step, and unit.
        -   enum: Enumeration. You must specify enumeration items with values and descriptions. For example, 1: Heating mode and 2: Cooling mode.
        -   bool: Boolean. You must specify the Boolean values. Values include 0 and 1. For example, you can use 0 to indicate disabled and 1 to indicate enabled.
        -   text: Text string. You must specify the data length. The maximum value is 2048 bytes.
        -   date: Timestamp. A UTC timestamp string, in milliseconds.
        -   struct: A JSON structure. Define a JSON structure, and add new parameters. For example, you can define that the color of a lamp as a structure composed of three parameters: red, green, and blue. Structure nesting is not supported.
        -   array: Array. You must select a data type for the elements in the array from int32, float, double, text, and struct. Make sure that the data type of elements in an array is the same and that the length of the array does not exceed 128 elements.
 **Note:** If the gateway connection protocol is Modbus, you do not set this parameter.

 |
        |Step|The smallest granularity of changes of properties, events, and input and output parameter values of services. If the data type is int32, float, or double, step is required.|
        |Unit|You can select None or a unit suitable.|
        |Read/Write Type|         -   Read/Write: GET and SET methods are supported for read/write requests.
        -   Read-only: Only GET is supported for read-only requests.
 **Note:** If the gateway connection protocol is Modbus, you do not need to set this parameter.

 |
        |Description|Enter a description or remarks about the property. You can enter up to 100 characters.|
        |Extended Information**Note:** The gateway connection protocol is Modbus.

|         -   Operation Type: You can select an operation type from Input Status \(read-only\), Coil Status \(read and write\), Holding Registers \(read and write\), and Input Registers \(read-only\).
        -   Register Address: Enter a hexadecimal address beginning with 0x, for example, 0xFE. The range is 0x0 - 0xFFFF.
        -   Original Data Type: Multiple data types are supported, including int16, uint16, int32, uint32, int64, uint64, float, double, string, bool, and customized data \(raw data\).
        -   Number of Registers: If the operation type is set to be Input Status \(read-only\) and Coil Status \(read and write\), the number range of registers is 1-2,000. If the operation type is Holding Registers \(read and write\) and Input Registers \(read-only\), the number range of registers is 1-125.
        -   Switch High Byte and Low Byte in Register: Swap the first 8 bits and the last 8 bits of the 16-bit data in the register.
        -   Switch Register Bits Sequence: Swap the bits of the original 32-bit data.
        -   Zoom Factor: The zoom factor is set to 1 by default. It can be set to negative numbers, but cannot be set to 0.
        -   Collection Interval: The time interval of data collection. It is in milliseconds and the value cannot be lower than 10.
        -   Data Report: The trigger of data report. It can be either At Specific Time or Report Changes.
 |
        |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node name, which must be unique in a property.|

    -   Define a service. In the Add Self-defined Feature dialog box, select **Services** as the feature type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154777736910856_en-US.png)

        The parameters of services are as follows.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Service name.

 A feature name must start with an English letter, Chinese character, or a number. It can contain English letters, Chinese characters, digits, dashes \(-\), and underscores \(\_\), and cannot exceed 30 characters in length.

 If you have selected a category with a feature template when you were creating the product, the system displays the standard services from the standard feature library for you to choose from.

 **Note:** If the gateway connection protocol is Modbus, you cannot define custom services for the product.

 |
        |Identifier|Identifies a service. It must be unique within the product. The parameter identifier in Alink JSON TSL. It is used as the key when this service is called. An identifier can contain English letters, digits, and underscores \(\_\), and cannot exceed 30 characters in length.**Note:** An identifier cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.

|
        |Invoke Method|         -   Asynchronous: For an asynchronous call, IoT Platform returns the result directly after the request is sent, and does not wait for a response from the device.
        -   Synchronous: For a synchronous call, IoT Platform waits for a response from the device. If no response is received, the call times out.
 |
        |Input Parameters|\(Optional\) Set input parameters for the service.Click **Add Parameter**, and add an input parameter in the dialog box that appears.

If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   You can either use a property as an input parameter or define an input parameter. For example, you can specify the properties Sprinkling Interval and Sprinkling Amount as the input parameters of the Automatic Sprinkler service feature. Then, when Automatic Sprinkler is called, the sprinkler automatically starts irrigation according to the sprinkling interval and amount.
        -   Identifiers of input parameters cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.
        -   You add up to 20 input parameters for a service.
|
        |Output Parameters|\(Optional\) Set output parameters for the service.Click **Add Parameter** , and add an output parameter in the dialog box that appears.

If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   You can either use a property as an output parameter or define an output parameter. For example, you can specify the property SoilHumidity as an output parameter. Then, when the service Automatic Sprinkler is called, IoT Platform returns the data about soil humidity.
        -   Identifiers of output parameters cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.
        -   You can add up to 20 output parameters for a service.
|
        |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node name, which must be unique in a service.|
        |Description|Enter a description or remarks about the service. You can enter up to 100 characters.|

    -   Define an event. In the Add Self-defined Feature dialog box, select **Events** as the feature type.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154777736910857_en-US.png)

        The parameters of events are as follows.

        |Parameter|Description|
        |:--------|:----------|
        |The function name| Event name.

 A feature name must start with a Chinese character, an English letter, or a digit, can contain Chinese characters, English letters, digits, dashes\(-\) and underscores \(\_\), and cannot exceed 30 characters in length.

 **Note:** If the gateway connection protocol is Modbus, you cannot define events.

 |
        |Identifier|Identifies an event. It must be unique in the product. It is the parameter identifier in Alink JSON TSL, and is used as the key when a device is reporting data of this event, for example, ErrorCode.**Note:** An identifier cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.

|
        |Event Type|         -   Info: Indicates general notifications reported by devices, such as the completion of a specific task.
        -   Alert: Indicates alerts that are reported by devices when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
        -   Error: Indicates errors that are reported by the device when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
 |
        |Output Parameters|The output parameters of an event. Click **Add Parameter**, and add an output parameter in the dialog box that appears. You can either use a property as an output parameter or define an output parameter. For example, you can specify the property Voltage as an output parameter. Then, devices report errors with the current voltage value for further fault diagnosis.If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

        -   Identifiers of output parameters cannot be any one of the following words: set, get, post, time, and value, because they are system parameter names.
        -   You can add up to 20 output parameters for an event.
|
        |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node name, which must be unique in an event.|
        |Description|Enter a description or remarks about the event. You can enter up to 100 characters.|


