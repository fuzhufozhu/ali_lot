# Overview {#concept_okp_zlv_tdb .concept}

Thing Specification Language \(TSL\) is a data model that digitizes a physical entity and constructs the entity data model in IoT Platform. In IoT Platform, a TSL model refers to a set of product features. After you have defined features for a product, the system automatically generates a TSL model of the product. A TSL model describes what a product is, what the product can do, and what services the product can provide.

A TSL model is a file in JSON format. TSL files are the digitized expressions of physical entities, such as sensors, vehicle-mounted devices, buildings and factories. A TSL file describes an entity in three dimensions: property \(what the entity is\), service \(what the entity can do\), and event \(what event information the entity reports\). Defining these three dimensions is to define the product features.

Therefore, the feature types of a product are Properties, Services and Events. You can define these three types of features in the console.

|Feature type|Description|
|:-----------|:----------|
|Property|Describes a running status of a device, such as the current temperature read by the environmental monitoring equipment. You can use GET and SET methods to send requests to get and set device properties.|
|Service|Indicates a feature or method of a device that can be used by a user. You can set input parameters and output parameters for a service. Compared with properties, services can implement more complex business logic, for example, a specific task.|
|Event|Indicates the notifications of a type of event occurred when a device is running. Events typically indicate notifications that require actions or attention, and they may contain multiple output parameters. For example, events can be notifications about the completion of tasks, system failures, or temperature alerts. You can subscribe to events or push events to a message receiving target.|

## The TSL format {#section_zvb_fdj_rox .section}

The format of Thing Specification Language \(TSL\) is JSON. This article introduces the JSON fields of TSL.

In the Define Feature tab of your target product, click **View TSL**.

The following section details each JSON field.

``` {#codeblock_nt1_t6b_wly}
{
    "schema":"TSL schema of a thing",
    "link":"System-level URI in the cloud, used to invoke services and subscribe to events",
    "profile":{
        "productKey":" Product ID",
    },
    "properties":[
        {
            "identifier":"Identifies a property. It must be unique under a product",
            "name":"Property name",
            "accessMode":"Read/write type of properties, including Read-Only and Read/Write",
            "required":"Determines whether a property that is required in the standard category is also required for a standard feature",
            "dataType":{
                "type":"Data type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
                "specs":{
                    "min":"Minimum value, available only for the int, float, and double data types",
                    "max":"Maximum value, available only for the int, float, and double data types",
                    "unit":"Property unit",
                    "unitName":"Unit name",
                    "size":"Array size, up to 128 elements, available only for the array data type",
                    "item":{
                        "type":"Type of an array element"
                    }
                }
            }
        }
    ],
    "events":[
        {
            "identifier":"Identifies an event that is unique under a product, where "post" are property events reported by default",
            "name":"Event name",
            "desc":"Event description",
            "type":"Event types, including info, alert, and error",
            "required":"Whether the event is required for a standard feature",
            "outputData":[
                {
                    "identifier":"Uniquely identifies a parameter",
                    "name":"Parameter name",
                    "dataType":{
                        "type":"Data type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
                        "specs":{
                            "min":"Minimum value, available only for the int, float, and double data types",
                            "max":"Maximum value, available only for the int, float, and double data types",
                            "unit":"Property unit",
                            "unitName":"Unit name",
                            "size":"Array size, up to 128 elements, available only for the array data type",
                            "item":{
                                "type":"Type of an array element"
                            }
                        }
                    }
                }
            ],
            "method":"Name of the method to invoke the event, generated according to the identifier"
        }
    ],
    "services":[
        {
            "identifier":"Identifies a service that is unique under a product (set and get are default services generated according to the read/write type of the property)",
            "name":"Service name",
            "desc":"Service description",
            "required":"Whether the service is required for a standard feature",
            "inputData":[
                {
                    "identifier":"Uniquely identifies an input parameter",
                    "name":"Name of an input parameter",
                    "dataType":{
                        "type":"Data type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
                        "specs":{
                            "min":"Minimum value, available only for the int, float, and double data types",
                            "max":"Maximum value, available only for the int, float, and double data types",
                            "unit":"Property unit",
                            "unitName":"Unit name",
                            "size":"Array size, up to 128 elements, available only for the array data type",
                            "item":{
                                "type":"Type of an array element"
                            }
                        }
                    }
                }
            ],
            "outputData":[
                {
                    "identifier":"Uniquely identifies an output parameter",
                    "name":"Name of an output parameter",
                    "dataType":{
                        "type":"Data type: int (original), float (original), double (original), text (original), date (UTC string in milliseconds), bool (integer, 0 or 1), enum (integer), struct (supports int, float, double, text, date, and bool), array (supports int, double, float, and text)",
                        "specs":{
                            "min":"Minimum value, available only for the int, float, and double data types",
                            "max":"Maximum value, available only for the int, float, and double data types",
                            "unit":"Property unit",
                            "unitName":"Unit name",
                            "size":"Array size, up to 128 elements, available only for the array data type",
                            "item":{
                                "type":"Type of an array element, available only for the array data type"
                            }
                        }
                    }
                }
            ],
            "method":"Name of the method to invoke the service, which is generated according to the identifier"
        }
    ]
}
```

If the product is connected to a gateway as a sub-device and the connection protocol is Modbus or OPC UA, you can view the TSL extension configuration.

``` {#codeblock_f7o_o4s_d2e}
{
"profile": {
"productKey": "Product ID",
  },
"properties": [
    {
"identifier": "Identifies a property. It must be unique under a product",
"operateType": "(coilStatus/inputStatus/holdingRegister/inputRegister)",
"registerAddress": "Register address",
"originalDataType": {
"type": "Data type: int16, uint16, int32, uint32, int64, uint64, float, double, string, customized data(returns hex data according to big-endian)",
"specs": {
"registerCount": "The number of registers, available only for string and customized data",
"swap16": "swap the first 8 bits and the last 8 bits of the 16 bits of the register data(for example, byte1byte2 -> byte2byte10). Available for all the other data types except string and customized data",
"reverseRegister": "Ex: Swap the bits of the original 32 bits data (for example, byte1byte2byte3byte4 ->byte3byte4byte1byte2”. Available for all the other data types except string and customized data"
        }
      },
"scaling": "Scaling factor",
"pollingTime": "Polling interval. The unit is ms",
"trigger": "The trigger of data report. Currently, two types of triggering methods are supported: 1: report at the specified time; 2: report when changes occurred"
    }
  ]
}

			
```

## Use TSL {#section_kvm_mnt_xfb .section}

1.  In the IoT Platform console, [Define features](reseller.en-US/User Guide/Create products and devices/TSL/Define TSL model/Define features.md#) or [Import Thing Specification Language \(TSL\)](reseller.en-US/User Guide/Create products and devices/TSL/Define TSL model/Import Thing Specification Language (TSL).md#).
2.  Develop the SDK.
3.  Connect the SDK to IoT Platform. Then, devices can report properties and events to IoT Platform, and in IoT Platform, you can set properties and call device services.

