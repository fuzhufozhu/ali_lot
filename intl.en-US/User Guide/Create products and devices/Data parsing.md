# Data parsing {#concept_rhj_535_42b .concept}

When you create a product on the IoT Platform console, if you select **Do not parse/Custom** as the data type, you can write a script in the IoT Platform console to parse the original data into Alink JSON format.

## What is data parsing? {#section_xq3_x35_42b .section}

Data parsing is a method that allows devices with limited storage space or bandwidth to avoid directly sending data to IoT Platform in Alink JSON format. Instead, devices pass original data to the cloud, whereby a script is run to convert the data into Alink JSON format. To allow devices to pass original data to the cloud, select Do not parse/Custom as the data type when creating the product, and then write a JavaScript file to parse the data. IoT Platform provides an online editor for you to edit and debug your data parsing script.

Data parsing process:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603317506_en-US.png)

Using the data parsing script editor, you can:

-   Edit your JavaScript data parsing file online.
-   Save content as a draft, edit the draft, or delete it.
-   Debug your script using analog data. You can enter upstream or downstream analog data, and run the script to check whether it works.
-   Perform static syntax check \(JavaScript syntax\).
-   Submit a verified script to the running platform for device data parsing.

## Edit a script online {#section_ar3_x35_42b .section}

On the product details page, click **Data Parsing** and then enter your data parsing script in the edit box. Currently, only JavaScript is supported.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603317507_en-US.png)

-   Click **Full Screen** to view or edit a script in full screen. Click **Exit Full Screen** to exit the full screen mode.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603327508_en-US.png)

-   Click **Save Draft** at the bottom of the page to save the content you have edited. When you access the data parsing page next time, the system will prompt a notification saying that you have a draft. You can then choose to **Restore Edit** or **Delete Draft**.

-   When saved, a draft script is not published to the running parsing platform, and does not affect a currently published script.
-   A new draft will overwrite any previously saved draft.
    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603327509_en-US.png)


## Verify the script using analog data {#section_hr3_x35_42b .section}

After the script is edited, you can enter analog data in the Analog Input box and click **Running**. The system will call this script to parse the analog data and the parsed result will be displayed in the Parsing Results box at the right side of the page. If the script is not correct, the message Failed to Run will be displayed next to Parsing Results, and an error message will be display in the box with information that you can use to to correct the script.

**Parse upstream analog data**

Select Upstreamed Device Data as the simulation type, enter the device's binary data which is to be passed through, and click **Running**. The system will convert the binary data to Alink JSON format, and the results are displayed in a box at the right side of the page.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603327510_en-US.png)

**Parse downstream analog data**

Select Receive Device Data, enter Alink JSON formatted data, and click **Running**. The system will convert the ALink JSON data to binary data, and the results are displayed in a box at the right side of the page.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603327511_en-US.png)

## Submit the script {#section_mr3_x35_42b .section}

In order to guarantee that submitted scripts are correct and run properly, only scripts that have passed parsing test can be submitted to the running platform. After a script is submitted, the system will automatically use it to convert the upstream data and downstream data of devices.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7527/15404603327512_en-US.png)

**Note:** A script must successfully parse analog data at least once before you can submit it.

## **Development framework** {#section_bkn_xj5_42b .section}

**Overview**

The following two methods must be defined in a script:

-   `protocolToRawData`: Convert Alink JSON format data to binary data.
-   `rawDataToProtocol`: Convert binary data to Alink JSON format data.

**Language**

Currently, only JavaScript that meets ECMAScript 5.1 is supported.

**Define the methods**

-   Convert Alink JSON formatted data to binary data:

    ```
    // Parses Alink JSON format data sent by the server and converts it to binary data
    function protocolToRawData(jsonObj){
        return rawdata;
    }
    ```

    Parameter description: Input parameters \(jsonObj\) match with the Alink JSON format data in the TSL of the product.

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

    Returned parameter: A binary byte array. For example:

    ```
    0x0100003039014d0142f6e76d
    ```

-   Convert binary data to Alink JSON format data:

    ```
    // Parses binary data sent by a device and converts it to Alink JSON format data
    function rawDataToProtocol(rawData){
        return jsonObj;
    }
    ```

    Parameter description: Input parameter \(rawData\) is a binary byte array, for example,

    ```
    0x00002233441232013fa00000
    ```

    Returned parameters: Data matches with the Alink JSON format data in the TSL of the product.

    ```
    {
        "method": "thing.event.property.post", 
        "id": "2241348", 
        "params": {
            "prop_float": 1.25, 
            "prop_int16": 4658, 
            "prop_bool": 1
        }, 
        "version": "1.0"
    }
    ```


## Script demo { .section}

1.  Create a product and define features for the product.
    1.  Create a Pro Edition product and select Do not parse/Custom as the data type.
    2.  Define features \(such as properties, services, and events\) for the product. In this demo, the following three properties are defined:

        |Identifier|Data type|
        |----------|---------|
        |prop\_float|float|
        |prop\_int16|int32|
        |prop\_bool|bool|

2.  Serial port protocol example.

    |Frame type|ID|prop\_int16|prop\_bool|prop\_float|
    |----------|--|-----------|----------|-----------|
    | One byte.

 0 - upstream; 1 - downstream.

 |Request sequence number.| Two bytes.

 Property value of prop\_int16.

 | One byte.

 Property value of prop\_bool.

 | Four bytes.

 Property value of prop\_float.

 |

3.  Copy the script demo codes.

    Copy and paste the following demo codes into the script edit box:

    ```
    var COMMAND_REPORT = 0x00;
    var COMMAND_SET = 0x01;
    var ALINK_PROP_REPORT_METHOD = 'thing.event.property.post'; //A standard ALink JSON formatted topic for devices to upload property data to the cloud.
    var ALINK_PROP_SET_METHOD = 'thing.service.property.set'; //A standard ALink JSON formatted topic for the cloud to send property management commands to devices.
    /*
    Sample data:
    Input parameters ->
        0x00002233441232013fa00000
    Output parameters  ->
        {"method":"thing.event.property.post","id":"2241348",
        "params":{"prop_float":1.25,"prop_int16":4658,"prop_bool":1},
        "version":"1.0"}
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
            jsonMap['method'] = ALINK_PROP_REPORT_METHOD; //ALink JSON formatted topic for reporting properties
            jsonMap['version'] = '1.0'; //Protocol version in ALink JSON format
            jsonMap['id'] = '' + dataView.getInt32(1); //The request ID value in ALink JSON format
            var params = {};
            params['prop_int16'] = dataView.getInt16(5); //The property of prop_int16 of the product
            params['prop_bool'] = uint8Array[7]; //The property of prop_bool
            params['prop_float'] = dataView.getFloat32(8); //The property of prop_float.
            jsonMap['params'] = params; //Standard fields of params in ALink JSON format
        }
        return jsonMap;
    }
    /*
    Sample data:
    Input parameters  ->
        {"method":"thing.service.property.set","id":"12345","version":"1.0","params":{"prop_float":123.452, "prop_int16":333, "prop_bool":1}}
    Output parameters ->
        0x0100003039014d0142f6e76d
    */
    function protocolToRawData(json) {
        var method = json['method'];
        var id = json['id'];
        var version = json['version'];
        var payloadArray = [];
        if (method == ALINK_PROP_SET_METHOD) // Property settings
        {
            var params = json['params'];
            var prop_float = params['prop_float'];
            var prop_int16 = params['prop_int16'];
            var prop_bool = params['prop_bool'];
            // Raw data connected according to the custom protocol format
            payloadArray = payloadArray.concat(buffer_uint8(COMMAND_SET)); // command field
            payloadArray = payloadArray.concat(buffer_int32(parseInt(id))); // ID in ALink JSON format
            payloadArray = payloadArray.concat(buffer_int16(prop_int16)); // The value of property 'prop_int16'
            payloadArray = payloadArray.concat(buffer_uint8(prop_bool)); // The value of property 'prop_bool'
            payloadArray = payloadArray.concat(buffer_float32(prop_float)); // The value of property 'prop_float'
        }
        return payloadArray;
    }
    // The followings are the auxiliary functions
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

4.  Verify the script using analog data
    -   Parse analog upstream data

        Select Upstreamed Device Data and enter the following data:

        ```
        0x00002233441232013fa00000
        ```

        click Running, and then view the outputs:

        ```
        {
            "method": "thing.event.property.post", 
            "id": "2241348", 
            "params": {
                "prop_float": 1.25, 
                "prop_int16": 4658, 
                "prop_bool": 1
            }, 
            "version": "1.0"
        }
        ```

    -   Select Received Device Data, and enter the following data:

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

        click Running, and then view the output:

        ```
        0x0100003039014d0142f6e76d
        ```


## Appendix: Method for debugging scripts written in a local computer { .section}

Currently, IoT Platform Data Parsing does not support debugging on the running platform. Therefore, we recommend that you directly paste the finished script into the online editor and then test it. The following is example output of the test method.

```
// Test Demo
function Test()
{
    //0x001232013fa00000
    var rawdata_report_prop = new Buffer([
        0x00, //Fixed command header, 0 indicates reporting property
        0x00, 0x22, 0x33, 0x44, // Identify the request sequence corresponding to the ID fields.
        0x12, 0x32, //Two-byte value in int16,  corresponding to the property of prop_int16
        0x01, //One-byte value in bool, corresponding to the property of prop_bool
        0x3f, 0xa0, 0x00, 0x00 //Four-byte value in float, corresponding to the property of prop_float
    ]);
    rawDataToProtocol(rawdata_report_prop);
    var setString = new String('{"method":"thing.service.property.set","id":"12345","version":"1.0","params":{"prop_float":123.452, "prop_int16":333, "prop_bool":1}}');
    protocolToRawData(JSON.parse(setString));
}
Test();
```

