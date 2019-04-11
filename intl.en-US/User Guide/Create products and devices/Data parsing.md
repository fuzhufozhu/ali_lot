# Data parsing {#concept_rhj_535_42b .concept}

Devices with low configurations and limited resources or devices that have high requirements for network traffic can send raw data to IoT Platform. This prevents the devices from directly sending data to IoT Platform in Alink JSON format. You must write a data parsing script in the IoT Platform console to parse upstream and downstream data to be in standard Alink JSON format and the custom data format, respectively.

## About data parsing {#section_xq3_x35_42b .section}

When receiving raw data from a device, IoT Platform runs the parsing script to convert the raw data to the Alink JSON data for business processing. When sending data to the device, IoT Platform also runs the parsing script to convert the Alink JSON data to the device custom formatted data.

Data parsing process:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15549506907506_en-US.png)

For more information about sending data upstream and downstream, see "Devices report properties or events" and "Call device services or set device properties" in [Communications over Alink protocol](../../../../../reseller.en-US/Developer Guide (Devices)/Develop devices based on Alink Protocol/Communications over Alink protocol.md#).

## Script format {#section_ihq_h2l_ngb .section}

```
/**
 * Convert data in Alink JSON format to data format that can be identified by the device. This feature is called when IoT Platform sends data to a device.
 *  Input: jsonObj   Object  Required
 *  Output: rawData byte[]   Array  Required
 *
 */
function protocolToRawData(jsonObj) {
    return rawdata;
}

/**
 * Convert the custom formatted data to Alink JSON data. This function is called when a device reports data to IoT Platform.
 * Input: rawData byte[]   Array   Required
 * Output: jsonObj   Object   Required   
 */
function rawDataToProtocol(rawData) {
    return jsonObj;
}
```

## Edit and verify scripts {#section_ar3_x35_42b .section}

Only JavaScript is supported to edit scripts. IoT Platform provides an online script editor that allows you to edit and submit scripts, and simulate data parsing for testing.

1.  Log on to the IoT Platform console.
2.  From the left-side navigation pane, choose **Devices** \> **Product**.
3.  Click **Create Product** to create a product and set the data type to **Do not parse/Custom**. For more information, see [Create a product](reseller.en-US/User Guide/Create products and devices/Create a product.md#).
4.  On the Product Details page, click the **Data Parsing** tab. Edit your data parsing script in the editor. Only JavaScript is supported. For more information, see [Example: Edit a script](#).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15549506907507_en-US.png)

    When you edit the script, you can perform the following operations:

    -   Click **Full Screen** to view or edit the script in full screen. Click **Exit Full Screen** to exit the full screen.

    -   Click**Save Draft** at the bottom of the page to save the content that you have edited. The next time you access the Data Parsing page, you will be notified that you have a draft. You can then choose to **restore edit** or **delete draft**.

-   A saved draft script will not be published to the running platform and will not affect a published script.
-   A new draft will overwrite any previously saved draft.
5.  After you finish editing the script, you can enter analog data in the **Analog Input** box. Click **Run** to test whether the script can be used to parse data correctly. For more information about analog data and parsing results, see [Verify a data parsing script](#).
6.  If you confirm that the script is correct and can parse data correctly, click **Submit** to submit the script to the running platform. When data is exchanged between IoT Platform and the device, the system will automatically call the corresponding function in the script to convert data.
7.  Perform a test by sending data to IoT Platform from a real device.

    1.  Register a device, and develop the [device SDK](../../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#)
    2.  The device connects to IoT Platform and reports data to IoT Platform.
    3.  In the IoT Platform console, go to the Device Details page of the device. Click the Status tab to view the device property data.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/155495069037537_en-US.png)


## Example: Edit a script {#scriptexample .section}

The following describes the data parsing script format and content of a product. In this example, the device data is in hexadecimal notation, and the product has three properties: prop\_float, prop\_int16, and prop\_bool.

1.  Create a product and select Do not parse/Custom as the data type. Then, define the following properties. For more information, see [Define features](reseller.en-US/User Guide/Create products and devices/TSL/Define features.md#).

    |Identifier|Type|Value range|Read/write|
    |:---------|:---|:----------|:---------|
    |prop\_float|float|-100 to 100|Read/write|
    |prop\_int16|int32|-100 to 100|Read/write|
    |prop\_bool|bool|0: Enabled. 1: Disabled.|Read/write|

2.  Define the communication protocol as follows:

    |Field|Number of bytes|
    |:----|:--------------|
    |Frame type|One|
    |Request ID|Four|
    |prop\_int16|Two|
    |prop\_bool|One|
    |prop\_float|Four|

    |Field|Number of bytes|
    |:----|:--------------|
    |Frame type|One|
    |Request ID|Four|
    |Result code|One|

    |Field|Number of bytes|
    |:----|:--------------|
    |Frame type|One|
    |Request ID|Four|
    |prop\_int16|Two|
    |prop\_bool|One|
    |prop\_float|Four|

    |Field|Number of bytes|
    |:----|:--------------|
    |Frame type|One|
    |Request ID|Four|
    |Result code|One|

3.  Edit the script.

    You must define the following methods in the script:

    -   `protocolToRawData`: Convert Alink JSON formatted data to custom formatted data.
    -   `rawDataToProtocol`: Convert custom formatted data to Alink JSON formatted data.
    A script demo is as follows:

    ```
    var COMMAND_REPORT = 0x00; //Devices report property
    var COMMAND_SET = 0x01; //Set property
    var COMMAND_REPORT_REPLY = 0x02; //Respond to the reported data
    var COMMAND_SET_REPLY = 0x03; //Respond to the property setting request
    var COMMAD_UNKOWN = 0xff;    //Other command
    var ALINK_PROP_REPORT_METHOD = 'thing.event.property.post'; //This is a topic for devices to report property data to IoT Platform.
    var ALINK_PROP_SET_METHOD = 'thing.service.property.set'; //This is a topic for for IoT Platform to send property management commands to devices.
    var ALINK_PROP_SET_REPLY_METHOD = 'thing.service.property.set'; //This is a topic for devices to report property setting results to IoT Platform.
    /*
    Sample data:
    Upstream data
    Input->
        0x000000000100320100000000
    Output->
        {"method":"thing.event.property.post","id":"1","params":{"prop_float":0,"prop_int16":50,"prop_bool":1},"version":"1.0"}
    
    Property setting response
    Input->
        0x0300223344c8
    Output->
        {"code":"200","data":{},"id":"2241348","version":"1.0"} 
    */
    function rawDataToProtocol(bytes) {
        var uint8Array = new Uint8Array(bytes.length);
        for (var i = 0; i < bytes.length; i++) {
            uint8Array[i] = bytes[i] & 0xff;
        }
        var dataView = new DataView(uint8Array.buffer, 0);
        var jsonMap = new Object();
        var fHead = uint8Array[0]; // command
        if (fHead == COMMAND_REPORT) {
            jsonMap['method'] = ALINK_PROP_REPORT_METHOD; //The Alink JSON formatted data topic for reporting properties
            jsonMap['version'] = '1.0'; //The fixed protocol version field in the Alink JSON format
            jsonMap['id'] = '' + dataView.getInt32(1); //The request ID in Alink JSON format
            var params = {};
            params['prop_int16'] = dataView.getInt16(5); //The value of prop_int16
            params['prop_bool'] = uint8Array[7]; //The value of prop_bool
            params['prop_float'] = dataView.getFloat32(8); //The value of prop_float
            jsonMap['params'] = params; //The value for params in Alink JSON format
        } else if(fHead == COMMAND_SET_REPLY) {
            jsonMap['version'] = '1.0'; //The fixed protocol version field in the Alink JSON format
            jsonMap['id'] = '' + dataView.getInt32(1); //The request ID value in Alink JSON format
            jsonMap['code'] = ''+ dataView.getUint8(5);
            jsonMap['data'] = {};
        }
    
        return jsonMap;
    }
    /*
    Sample data:
    Property setting
    Input->
        {"method":"thing.service.property.set","id":"12345","version":"1.0","params":{"prop_float":123.452, "prop_int16":333, "prop_bool":1}}
    Output->
        0x0100003039014d0142f6e76d
    
    Upstream data response
    Input->
        {"method":"thing.event.property.post","id":"12345","version":"1.0","code":200,"data":{}}
    Output->
        0x0200003039c8
    */
    function protocolToRawData(json) {
        var method = json['method'];
        var id = json['id'];
        var version = json['version'];
        var payloadArray = [];
        if (method == ALINK_PROP_SET_METHOD) //Set properties
        {
            var params = json['params'];
            var prop_float = params['prop_float'];
            var prop_int16 = params['prop_int16'];
            var prop_bool = params['prop_bool'];
            //Join raw data according to the custom protocol format
            payloadArray = payloadArray.concat(buffer_uint8(COMMAND_SET)); //The command field
            payloadArray = payloadArray.concat(buffer_int32(parseInt(id))); //The ID in Alink JSON format
            payloadArray = payloadArray.concat(buffer_int16(prop_int16)); //The value of prop_int16
            payloadArray = payloadArray.concat(buffer_uint8(prop_bool)); //The value of prop_bool
            payloadArray = payloadArray.concat(buffer_float32(prop_float)); //The value of prop_float
        } else if (method ==  ALINK_PROP_REPORT_METHOD) { //Response to device upstream data
            var id = json['id'];
            payloadArray = payloadArray.concat(buffer_uint8(COMMAND_SET)); // The command field
            payloadArray = payloadArray.concat(buffer_int32(parseInt(id))); //The ID in Alink JSON format
            payloadArray = payloadArray.concat(buffer_uint8(code));
        } else { //Other commands that will not be processed
            var id = json['id'];
            payloadArray = payloadArray.concat(buffer_uint8(COMMAND_SET)); //The command field
            payloadArray = payloadArray.concat(buffer_int32(parseInt(id))); //The ID in Alink JSON format
            payloadArray = payloadArray.concat(buffer_uint8(code));
        }
        return payloadArray;
    }
    //The following lists some auxiliary functions:
    function buffer_uint8(value) {
        var uint8Array = new Uint8Array(1);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setUint8(0, value);
        return [].slice.call(uint8Array);
    }
    function buffer_int16(value) {
        var uint8Array = new Uint8Array(2);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setInt16(0, value);
        return [].slice.call(uint8Array);
    }
    function buffer_int32(value) {
        var uint8Array = new Uint8Array(4);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setInt32(0, value);
        return [].slice.call(uint8Array);
    }
    function buffer_float32(value) {
        var uint8Array = new Uint8Array(4);
        var dv = new DataView(uint8Array.buffer, 0);
        dv.setFloat32(0, value);
        return [].slice.call(uint8Array);
    }
    ```


## Verify a data parsing script {#debugscript .section}

After you edit a sample script, you can verify the correctness of the script. Enter analog data in the Analog Input box, and click **Run**. The system will call this script to parse the analog data. The parsed result will be displayed in the Parsing Results box at the right side of the page.

-   Parse the device-reported property data

    Select Upstreamed Device Data as the simulation type, enter the following hexadecimal data, and then click **Run**.

    ```
    0x00002233441232013fa00000
    ```

    The data parsing engine will convert the hexadecimal data to JSON data as defined in the script. The result will be displayed in the **Parsing Results** area.

    ```
    {
        "method": "thing.event.property.post", 
        "id": "2241348", 
        "params": {
            "prop_float": 1.25, 
            "prop_int16": 4658, 
            "prop_bool": 1
        }, 
        "version": "1.0",
    }
    ```

-   Parse downstream data from IoT Platform to the device.

    Select **Received Device Data** as the simulation type, enter the following JSON data, and then click **Run**.

    ```
    {
      "id": "12345",
      "version": "1.0",
      "code": 200,
      "method": "thing.event.property.post",
      "data": {}
    }
    ```

    The data parsing engine will convert the JSON data to the following hexadecimal data.

    ```
    0x0200003039c8
    ```

-   Parse the property setting data from IoT Platform to devices.

    Select Received Device Data as the simulation type, enter the following JSON data, and then click **Run**.

    ```
    {
        "method": "thing.service.property.set", 
        "id": "12345", 
        "version": "1.0", 
        "params": {
            "prop_float": 123.452, 
            "prop_int16": 333, 
            "prop_bool": 1
        }
    }
    ```

    The data parsing engine converts JSON data to the following hexadecimal data.

    ```
    0x0100003039014d0142f6e76d
    ```

-   Parse property setting results returned by the device.

    Select Upstreamed Device Data as the simulation type, enter the following hexadecimal data, and then click **Run**.

    ```
    0x0300223344c8
    ```

    The data parsing engine will convert the hexadecimal data to the following JSON data.

    ```
    {
      "code": "200",
      "data": {}
      "id": "2241348",
      "version": "1.0",
    }
    ```


If the script is incorrect, an error message is displayed in the **Parsing Results** area. You must troubleshoot the error according to the error message and modify the script code accordingly.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/155495069037533_en-US.png)

## Debug a data parsing script in a local computer {#section_krw_cwk_ngb .section}

IoT Platform Data Parsing does not support debugging on the running platform. We recommend that you develop and debug the script locally and then paste the finished script into the online editor. You may use the following debugging method.

```
// Test Demo
function Test()
{
    //0x001232013fa00000
    var rawdata_report_prop = new Buffer([
        0x00, //The fixed command header. A value of 0 indicates a property report message.
        0x00, 0x22, 0x33, 0x44, // The ID fields that identify the request sequence.
        0x12, 0x32, //Two-byte value of prop_int16
        0x01, //One-byte value of prop_bool
        0x3f, 0xa0, 0x00, 0x00 //Four-byte value of prop_float
    ]);
    rawDataToProtocol(rawdata_report_prop);
    var setString = new String('{"method":"thing.service.property.set","id":"12345","version":"1.0","params":{"prop_float":123.452, "prop_int16":333, "prop_bool":1}}'); 
    protocolToRawData(JSON.parse(setString));
}
Test();
```

## Troubleshoot issues {#section_gc2_p5b_mgb .section}

After a device is connected to IoT Platform and reports data, the reported data can be displayed in the IoT console if data parsing functions correctly. To view the data, go to the Device Details page of the device and click the Status tab.

In some occasions, after the device reports data, no data is displayed on the page, as shown in the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/155495069037538_en-US.png)

To view device logs: From the left-side navigation pane, choose **Maintenance** \> **Device Log** and select the corresponding product. On the Device Log page, click the **TSL Data Analysis** tab. You can view the communication log between the device and IoT Platform.

Use the following process to troubleshoot the issue:

1.  View the reported data on the Device Log page. Each log entry records the converted data and the original data.
2.  Check the error codes according to the descriptions in [Device log](reseller.en-US/User Guide/Monitoring and Maintenance/Device log.md#).
3.  Troubleshoot the issue based on the error code, the script, and the reported data.

The following lists some errors:

-   The data parsing script is not found.

    As shown in the following figure, the error code is 6200. To check the description of the error, see [Device log](reseller.en-US/User Guide/Monitoring and Maintenance/Device log.md#). The error code of 6200 indicates that no script was found. Check whether the data parsing script has been submitted in the console.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/155495069037539_en-US.png)

-   Alink method does not exist.

    The error code is 6450. This error code is described in [Device log](reseller.en-US/User Guide/Monitoring and Maintenance/Device log.md#) as follows: The method parameter is not found in Alink data. This error occurs if the method parameter is not found in the Alink data reported by the device or in the parsed result of Do not parse/Custom data.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/155495069037540_en-US.png)

    You can check the raw data, for example:

    ```
    17:54:19.064, A7B02C60646B4D2E8744F7AA7C3D9567, upstream-error - bizType=OTHER_MESSAGE,params={"params":{}},result=code:6450,message:alink method not exist,...
    ```

    In the log, the error message is `alink method not exist`. If this error occurs, you must correct your script.


