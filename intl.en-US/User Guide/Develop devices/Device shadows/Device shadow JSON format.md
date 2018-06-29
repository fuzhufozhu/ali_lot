# Device shadow JSON format {#concept_w3l_41v_wdb .concept}

## Format of the device shadow JSON file {#section_ssc_bcx_wdb .section}

The format is as follows:

```

{
"state": {
"desired": {
"attribute1": integer2,
"attribute2": "string2",
...
"attributeN": boolean2
},
"reported": {
"attribute1": integer1,
"attribute2": "string1",
...
"attributeN": boolean1
}
},
"metadata": {
"desired": {
"attribute1": {
"timestamp": timestamp
},
"attribute2": {
"timestamp": timestamp
},
...
"attributeN": {
"timestamp": timestamp
}
},
"reported": {
"attribute1": {
"timestamp": timestamp
},
"attribute2": {
"timestamp": timestamp
},
...
"attributeN": {
"timestamp": timestamp
}
}
},
"timestamp": timestamp,
"version": version
}
```

The JSON properties are described in [Table 1](#table_mwk_fcx_wdb).

|Property|Description|
|--------|-----------|
|desired|The desired status of the device.The application writes the desired property of the device, without accessing the device.

|
|reported|The status that the device has reported. The device writes data to the reported property to report its latest status.The application obtains the status of the device by reading this property.

|
|metadata|The device shadow service automatically updates metadata according to the updates in the device shadow JSON file.State metadata in the device shadow JSON file contains the timestamp of each property. The timestamp is represented as epoch time to obtain exact update time.

|
|timestamp|The latest update time of the device shadow JSON file.|
|version|When you request updating the version of the device shadow, the device shadow checks whether the requested version is later than the current version.If the requested version is later than the current one, the device shadow updates to the requested version. If not, the device shadow rejects the request.

The version number is increased according to the version update to ensure the latest device shadow JSON file version.

|

Example of the device shadow JSON file:

```

{
"state" : {
"desired" : {
"color" : "RED",
"sequence" : [ "RED", "GREEN", "BLUE" ]
},
"reported" : {
"color" : "GREEN"
}
},
"metadata" : {
"desired" : {
"color" : {
"timestamp" : 1469564492
},
"sequence" : {
"timestamp" : 1469564492
}
},
"reported" : {
"color" : {
"timestamp" : 1469564492
}
}
},
"timestamp" : 1469564492,
"version" : 1
}
```

## Empty properties {#section_aly_cdx_wdb .section}

-   The device shadow JSON file contains the desired property only when you have specified the desired status. The following device shadow JSON file, which does not contain the desired property, is also effective:

    ```
    
    {
    "state" : {
    "reported" : {
    "color" : "red",
    }
    },
    "metadata" : {
    "reported" : {
    "color" : {
    "timestamp" : 1469564492
    }
    }
    },
    "timestamp" : 1469564492,
    "version" : 1
    }
    ```

-   The following device shadow JSON file, which does not contain the reported property, is also effective:

    ```
    
    {
    "state" : {
    "desired" : {
    "color" : "red",
    }
    },
    "metadata" : {
    "desired" : {
    "color" : {
    "timestamp" : 1469564492
    }
    }
    },
    "timestamp" : 1469564492,
    "version" : 1
    }
    ```


## Array {#section_chh_ldx_wdb .section}

The device shadow JSON file can use an array, and must update this array as a whole when the update is required.

-   Initial status:

    ```
    
    {
    "reported" : { "colors" : ["RED", "GREEN", "BLUE" ] }
    }
    ```

-   Update:

    ```
    
    {
    "reported" : { "colors" : ["RED"] }
    }
    ```

-   Final status:

    ```
    
    {
    "reported" : { "colors" : ["RED"] }
    }
    ```


