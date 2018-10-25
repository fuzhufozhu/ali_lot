# Define features using TSL {#task_qhm_d3j_w2b .task}

This article introduce how to define features in the IoT Platform console.

1.  Log on to the [IoT Platform console](https://partners-intl.console.aliyun.com/#/iot). 
2.  In the left-side navigation pane, click Products, find the Pro Edition product for which you want to add features, and then click **View** next to it. 
3.  Click **Define Feature** \> **Add**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154046018910854_en-US.png)

4.   In the Add Feature dialog box, select **Properties** as the feature type. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154046018910855_en-US.png)

    The parameters of properties are listed in the following table.

    |Parameter|Description|
    |---------|-----------|
    |The function name|The name of the property, for example, Power Consumption. Each feature name under a product must be unique. If you have selected a feature template during product creation, the system displays standard properties from the standard feature library for you to choose.**Note:** If the gateway connection protocol is Modbus, standard properties are not supported. Instead, you must define properties manually.

|
    |Identifier|Identifies a property. It must be unique under a product. The properties parameter is identifier in Alink JSON, and is used as the key when a device is reporting the data of this property. Specifically, the cloud uses this parameter to verify the identifier and determine whether to receive the data. An identifier can contain letters, numbers, and underscores \(\_\), for example, Power\_Consumption1.|
    |Data Type|     -   Int32: 32-bit integer. You must define the value range, resolution, and unit.
    -   float: Float. You must define the value range, resolution, and unit.
    -   double: Double. You must define the value range, resolution, and unit.
    -   enum: Enumeration. You must specify enumeration items with values and descriptions. For example,1 indicates Heating mode and 2 indicates Cooling mode.
    -   bool: Boolean. You must specify the Boolean value. Values include 0 and 1. You can use 0 to indicate Disabled and 1 to indicate Enabled.
    -   text: Text string. You must specify the data length. The maximum value is 2048 bytes.
    -   data: Timestamp. A UTC timestamp in string type, in milliseconds.
    -   struct: A JSON target Define a JSON structure, and add new JSON parameters. For example, you can define that the color of a lamp is a structure composed of three parameters: red, green, and blue. Structure nesting is not supported.
    -   array: Array. You must select a data type for the elements in the array from **int32**, **float**, **double**, and **text**. Make sure that the data type of elements in an array is the same and that the length of the array does not exceed 128 elements.
 **Note:** If the gateway connection protocol is Modbus, you do not need to set this parameter.

 |
    |Read/Write Type|     -   Read/Write: GET and SET methods are supported for Read/Write requests.
    -   Read-only: Only GET is supported for Read-only requests.
 **Note:** If the gateway connection protocol is Modbus, you do not need to set this parameter.

 |
    |Description |Enter a description or remarks about the feature.|
    |Extended Information**Note:** The gateway connection protocol is Modbus.

|     -   Operation Type: You can select an operation type from Input Status \(read-only\), Coil Status \(read and write\), Holding Registers \(read and write\), and Input Registers \(read-only\).
    -   Register Address: Enter a hexadecimal address beginning with 0x, for example, 0xFE. The range is 0x0-0xFFFF.
    -   Original Data Type: Multiple data types are supported, including int16, uint16, int32, uint32, int64, uint64, float, double, string, bool, and customized data \(raw data\).
    -   Number of Registers: If the operation type is set to be Input Status \(read-only\) and Coil Status \(read and write\), the number range of registers is 1-2,000. If the operation type is Holding Registers \(read and write\) and Input Registers \(read-only\), the number range of registers is 1-125.
    -   Switch High Byte and Low Byte in Register: Swap the first 8 bits and the last 8 bits of the 16-bit data in the register.
    -   Switch Register Bits Sequence: Swap the bits of the original 32-bit data.
    -   Zoom Factor: The zoom factor is set to 1 by default. It can be set to negative numbers, but cannot be set to 0.
    -   Collection Interval: The time interval of data collection. It is in milliseconds and the value cannot be lower than 10.
    -   Data Report: The trigger of data report. It can be either At Specific Time or Report Changes.
 |
    |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node Name: Each node name must be unique under the property|

5.   In the Add Feature dialog box, select the feature type as **Services**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154046018910856_en-US.png)

    The parameters of a service are as follows.

    |Parameter|Description |
    |---------|------------|
    |The function name|Service name. If you have selected a feature template during product creation, the system displays standard services from the standard feature library for you to choose.**Note:** If the gateway connection protocol is Modbus, you cannot define custom services for the product.

|
    |Identifier|Identifies a service. It must be unique under a product. The service parameter is identifier in Alink JSON, and is used as the key when a device is reporting the data of this feature. The identifier can contain letters, numbers, and underscores \(\_\).|
    |Invoke Method|     -   Asynchronous: For an asynchronous call, the cloud returns the result directly after the call is executed instead of waiting for responses from the device.
    -   Synchronous: For a synchronous call, the cloud waits for the response from the device. If no responses are received, the call times out.
 |
    |Input Parameters|\(Optional\) Set input parameters for the service.Click **Add Parameter**, and add an input parameter in the dialog box that appears. For more information about input parameters, see [step 4](#table_odh_x43_y2b).

If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

    -   You can either use a property as an input parameter or define an input parameter. For example, you can specify the properties Sprinkling Interval and Sprinkling Amount as the input parameters of the Automatic Sprinkler service feature. Then, when Automatic Sprinkler is called, the sprinkler automatically starts irrigation according to the sprinkling interval and amount.
    -   A service supports a maximum of 10 input parameters.
|
    |Output Parameters|\(Optional\) Set output parameters for the service.Click **Add Parameter**, and add an output parameter in the dialog box that appears. For more information about input parameters, see [step 4](#table_odh_x43_y2b).

If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** 

    -   You can either use a property as an output parameter or define an output parameter. For example, you can specify the property Soil Humidity as an output parameter. Then, when Automatic Sprinkler is called, the cloud returns the data about soil humidity.
    -   A service supports a maximum of 10 output parameters.
|
    |Description|Enter a description or remarks about the service.|
    |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node Name: Each node name must be unique under the service.|

6.   In the Add Feature dialog box, select the feature type as **Events**. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17782/154046018910857_en-US.png)

    The parameters of an event are as follows.

    |Parameter|Description |
    |---------|------------|
    |The function name|Enter an event name.**Note:** If the gateway connection protocol is Modbus, you cannot define events.

|
    |Identifier|Identifies an event. It must be unique under a product. The event parameter is identifier in Alink JSON, and is used as the key when the device is reporting the data of this feature. For example, ErrorCode.|
    |Event Type|     -   Info: Indicates general notifications reported by the device, such as the completion of a specific task.
    -   Alert: Indicates alerts that are reported by the device when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
    -   Error: Indicates errors that are reported by the device when unexpected or abnormal events occur. It has a high priority. You can perform logic processing or analytics depending on the event type.
 |
    |Output Parameters|The output parameters of the event. Click **Add Parameter**. You can either use a property as an output parameter or define an output parameter. For example, you can specify the property Voltage as an output parameter. Then, the device reports the error with the current voltage value for further fault diagnosis.If the gateway connection protocol is OPC UA, you must set the parameter index that is used to mark the order of the parameters.

**Note:** An event supports a maximum of 10 output parameters.

|
    |Description|Enter a description or remarks about the event.|
    |Extended Information**Note:** The gateway connection protocol is OPC UA.

|Node Name: Each node name must be unique under the event.|


