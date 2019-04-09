# Send configuration data to gateway devices {#concept_n1j_cc4_2fb .concept}

Send extended configuration information of the TSL model and sub-device connection channel configuration that you configured on the cloud to the gateway device.

## Send configuration data {#section_wnw_vsf_jhb .section}

Request topic: `/sys/{productKey}/{deviceName}/thing/model/config/push`

Request message

```
{
  "id": 123,
  "version": "1.0",
  "method": "thing.model.config.push",
  "data": {
    "digest":"",
    "digestMethod":"",
    "url": ""
  }
}
```

​Parameter description​

|Parameter|Type |Description|
|---------|-----|-----------|
|id|String|The message ID. IoT Platform generates a message ID for a downstream message.|
|version|String|The protocol version number. Default value: 1.0.|
|method|String|The method is `thing.model.config.push`.|
|data|Object|Data|
|digest|String|The signature that is used to verify the integrity of the data obtained from url.|
|digestMethod|String|The signature method. The default method is sha256.|
|url|String|The URL where the configuration data is stored.|

Data from the URL:

```
{
  "modelList": [
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "services": [
        {
          "outputData": "", 
          "identifier": "AngleSelfAdaption", 
          "inputData": [
            {
              "identifier": "test01", 
              "index": 0
            }
          ], 
          "displayName": "test01"
        }
      ], 
      "properties": [
        {
          "identifier": "identifier", 
          "displayName": "test02"
        }, 
        {
          "identifier": "identifier_01", 
          "displayName": "identifier_01"
        }
      ], 
      "events": [
        {
          "outputData": [
            {
              "identifier": "test01", 
              "index": 0
            }
          ], 
          "identifier": "event1", 
          "displayName": "abc"
        }
      ]
    }, 
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "properties": [
        {
          "originalDataType": {
            "specs": {
              "registerCount": 1, 
              "reverseRegister": 0, 
              "swap16": 0
            }, 
            "type": "bool"
          }, 
          "identifier": "test01", 
          "registerAddress": "0x03", 
          "scaling": 1, 
          "operateType": "inputStatus", 
          "pollingTime": 1000, 
          "trigger": 1
        }, 
        {
          "originalDataType": {
            "specs": {
              "registerCount": 1, 
              "reverseRegister": 0, 
              "swap16": 0
            }, 
            "type": "bool"
          }, 
          "identifier": "test02", 
          "registerAddress": "0x05", 
          "scaling": 1, 
          "operateType": "coilStatus", 
          "pollingTime": 1000, 
          "trigger": 2
        }
      ]
    }, 
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "properties": [
        {
          "identifier": "test_02", 
          "customize": {
            "test_02": 123
          }
        }, 
        {
          "identifier": "test_01", 
          "customize": {
            "test01": 1
          }
        }
      ]
    }
  ], 
  "serverList": [
    {
      "baudRate": 1200, 
      "protocol": "RTU", 
      "byteSize": 8, 
      "stopBits": 2, 
      "parity": 1, 
      "name": "modbus01", 
      "serialPort": "0", 
      "serverId": "D73251B427****"
    }, 
    {
      "protocol": "TCP", 
      "port": 8000, 
      "ip": "192.168.0.1", 
      "name": "modbus02", 
      "serverId": "586CB066D****"
    }, 
    {
      "password": "XIJTginONohPEUAyZ****==", 
      "secPolicy": "Basic128Rsa15", 
      "name": "server_01", 
      "secMode": "Sign", 
      "userName": "123", 
      "serverId": "55A9D276A7E****", 
      "url": "tcp:00", 
      "timeout": 10
    }, 
    {
      "password": "hAaX5s13gwX2JwyvUk****==", 
      "name": "service_09", 
      "secMode": "None", 
      "userName": "1234", 
      "serverId": "44895C63E3F****", 
      "url": "tcp:00", 
      "timeout": 10
    }
  ], 
  "deviceList": [
    {
      "deviceConfig": {
        "displayNamePath": "123", 
        "serverId": "44895C63E3FF4013924CEF31519A****"
      }, 
      "productKey": "a1ZlGQv****", 
      "deviceName": "test_02"
    }, 
    {
      "deviceConfig": {
        "displayNamePath": "1", 
        "serverId": "55A9D276A7****"
      }, 
      "productKey": "a1ZlGQv****", 
      "deviceName": "test_03"
    }, 
    {
      "deviceConfig": {
        "slaveId": 1, 
        "serverId": "D73251B4277****"
      }, 
      "productKey": "a1ZlGQv****", 
      "deviceName": "test01"
    }, 
    {
      "deviceConfig": {
        "slaveId": 2, 
        "serverId": "586CB066D6****"
      }, 
      "productKey": "a1ZlGQv****", 
      "deviceName": "test02"
    }
  ], 
  "tslList": [
    {
      "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json", 
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "services": [
        {
          "outputData": [ ], 
          "identifier": "set", 
          "inputData": [
            {
              "identifier": "test02", 
              "dataType": {
                "specs": {
                  "unit": "mm", 
                  "min": "0", 
                  "max": "1"
                }, 
                "type": "int"
              }, 
              "name": "test02"
            }
          ], 
          "method": "thing.service.property.set", 
          "name": "set", 
          "required": true, 
          "callType": "async", 
          "desc": "set property"
        }, 
        {
          "outputData": [
            {
              "identifier": "test01", 
              "dataType": {
                "specs": {
                  "unit": "m", 
                  "min": "0", 
                  "max": "1"
                }, 
                "type": "int"
              }, 
              "name": "test01"
            }, 
            {
              "identifier": "test02", 
              "dataType": {
                "specs": {
                  "unit": "mm", 
                  "min": "0", 
                  "max": "1"
                }, 
                "type": "int"
              }, 
              "name": "test02"
            }
          ], 
          "identifier": "get", 
          "inputData": [
            "test01", 
            "test02"
          ], 
          "method": "thing.service.property.get", 
          "name": "get", 
          "required": true, 
          "callType": "async", 
          "desc": "get property"
        }
      ], 
      "properties": [
        {
          "identifier": "test01", 
          "dataType": {
            "specs": {
              "unit": "m", 
              "min": "0", 
              "max": "1"
            }, 
            "type": "int"
          }, 
          "name": "test01", 
          "accessMode": "r", 
          "required": false
        }, 
        {
          "identifier": "test02", 
          "dataType": {
            "specs": {
              "unit": "mm", 
              "min": "0", 
              "max": "1"
            }, 
            "type": "int"
          }, 
          "name": "test02", 
          "accessMode": "rw", 
          "required": false
        }
      ], 
      "events": [
        {
          "outputData": [
            {
              "identifier": "test01", 
              "dataType": {
                "specs": {
                  "unit": "m", 
                  "min": "0", 
                  "max": "1"
                }, 
                "type": "int"
              }, 
              "name": "test01"
            }, 
            {
              "identifier": "test02", 
              "dataType": {
                "specs": {
                  "unit": "mm", 
                  "min": "0", 
                  "max": "1"
                }, 
                "type": "int"
              }, 
              "name": "test02"
            }
          ], 
          "identifier": "post", 
          "method": "thing.event.property.post", 
          "name": "post", 
          "type": "info", 
          "required": true, 
          "desc": "report property"
        }
      ]
    }, 
    {
      "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json", 
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "services": [
        {
          "outputData": [ ], 
          "identifier": "set", 
          "inputData": [
            {
              "identifier": "identifier", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "7614"
            }, 
            {
              "identifier": "identifier_01", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "test1"
            }
          ], 
          "method": "thing.service.property.set", 
          "name": "set", 
          "required": true, 
          "callType": "async", 
          "desc": "set property"
        }, 
        {
          "outputData": [
            {
              "identifier": "identifier", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "7614"
            }, 
            {
              "identifier": "identifier_01", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "test1"
            }
          ], 
          "identifier": "get", 
          "inputData": [
            "identifier", 
            "identifier_01"
          ], 
          "method": "thing.service.property.get", 
          "name": "get", 
          "required": true, 
          "callType": "async", 
          "desc": "get property"
        }, 
        {
          "outputData": [ ], 
          "identifier": "AngleSelfAdaption", 
          "inputData": [
            {
              "identifier": "test01", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "10", 
                  "step": "1"
                }, 
                "type": "int"
              }, 
              "name": "param1"
            }
          ], 
          "method": "thing.service.AngleSelfAdaption", 
          "name": "angleadjust", 
          "required": false, 
          "callType": "async"
        }
      ], 
      "properties": [
        {
          "identifier": "identifier", 
          "dataType": {
            "specs": {
              "length": "2048"
            }, 
            "type": "text"
          }, 
          "name": "7614", 
          "accessMode": "rw", 
          "required": true
        }, 
        {
          "identifier": "identifier_01", 
          "dataType": {
            "specs": {
              "length": "2048"
            }, 
            "type": "text"
          }, 
          "name": "test1", 
          "accessMode": "rw", 
          "required": false
        }
      ], 
      "events": [
        {
          "outputData": [
            {
              "identifier": "identifier", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "7614"
            }, 
            {
              "identifier": "identifier_01", 
              "dataType": {
                "specs": {
                  "length": "2048"
                }, 
                "type": "text"
              }, 
              "name": "test1"
            }
          ], 
          "identifier": "post", 
          "method": "thing.event.property.post", 
          "name": "post", 
          "type": "info", 
          "required": true, 
          "desc": "report property"
        }, 
        {
          "outputData": [
            {
              "identifier": "test01", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "20", 
                  "step": "1"
                }, 
                "type": "int"
              }, 
              "name": "param1"
            }
          ], 
          "identifier": "event1", 
          "method": "thing.event.event1.post", 
          "name": "event1", 
          "type": "info", 
          "required": false
        }
      ]
    }, 
    {
      "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json", 
      "profile": {
        "productKey": "a1ZlGQv****"
      }, 
      "services": [
        {
          "outputData": [ ], 
          "identifier": "set", 
          "inputData": [
            {
              "identifier": "test_01", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "1"
                }, 
                "type": "int"
              }, 
              "name": "param1"
            }, 
            {
              "identifier": "test_02", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "10"
                }, 
                "type": "double"
              }, 
              "name": "param2"
            }
          ], 
          "method": "thing.service.property.set", 
          "name": "set", 
          "required": true, 
          "callType": "async", 
          "desc": "set property"
        }, 
        {
          "outputData": [
            {
              "identifier": "test_01", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "1"
                }, 
                "type": "int"
              }, 
              "name": "param1"
            }, 
            {
              "identifier": "test_02", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "10"
                }, 
                "type": "double"
              }, 
              "name": "param2"
            }
          ], 
          "identifier": "get", 
          "inputData": [
            "test_01", 
            "test_02"
          ], 
          "method": "thing.service.property.get", 
          "name": "get", 
          "required": true, 
          "callType": "async", 
          "desc": "get property"
        }
      ], 
      "properties": [
        {
          "identifier": "test_01", 
          "dataType": {
            "specs": {
              "min": "1", 
              "max": "100", 
              "step": "1"
            }, 
            "type": "int"
          }, 
          "name": "param1", 
          "accessMode": "rw", 
          "required": false
        }, 
        {
          "identifier": "test_02", 
          "dataType": {
            "specs": {
              "min": "1", 
              "max": "100", 
              "step": "10"
            }, 
            "type": "double"
          }, 
          "name": "param2", 
          "accessMode": "rw", 
          "required": false
        }
      ], 
      "events": [
        {
          "outputData": [
            {
              "identifier": "test_01", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "1"
                }, 
                "type": "int"
              }, 
              "name": "param1"
            }, 
            {
              "identifier": "test_02", 
              "dataType": {
                "specs": {
                  "min": "1", 
                  "max": "100", 
                  "step": "10"
                }, 
                "type": "double"
              }, 
              "name": "param2"
            }
          ], 
          "identifier": "post", 
          "method": "thing.event.property.post", 
          "name": "post", 
          "type": "info", 
          "required": true, 
          "desc": "report property"
        }
      ]
    }
  ]
}
```

Parameters in the data:

|Parameter|Type |Description|
|---------|-----|-----------|
|modelList|Object|The extended product information of all sub-devices that are mounted to the gateway. For more information, see the following section [modelList description](#).|
|serverList|Object|The sub-device channels of the gateway. For more information, see the following section [serverList description](#).|
|deviceList|Object|The connection configurations of all sub-devices that are mounted to the gateway. For more information, see the following section [deviceList description](#).|
|tslList|Object|The TSL of all sub-devices that are mounted to the gateway.|

## modelList description {#section_ctm_tvs_2fb .section}

Currently, the communication protocols Modbus and OPC UA, and custom protocol are supported. The extended information are different when using different protocols.

-   When the protocol is Modbus, the extended product information of sub-devices is as the following:

    ```
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      },
      "properties": [
        {
          "originalDataType": {
            "specs": {
              "registerCount": 1,
              "reverseRegister": 0,
              "swap16": 0
            },
            "type": "bool"
          },
          "identifier": "test01",
          "registerAddress": "0x03",
          "scaling": 1,
          "operateType": "inputStatus",
          "pollingTime": 1000,
          "trigger": 1
        },
        {
          "originalDataType": {
            "specs": {
              "registerCount": 1,
              "reverseRegister": 0,
              "swap16": 0
            },
            "type": "bool"
          },
          "identifier": "test02",
          "registerAddress": "0x05",
          "scaling": 1,
          "operateType": "coilStatus",
          "pollingTime": 1000,
          "trigger": 2
        }
      ]
    }
    ```

    Parameter description

    |Parameter|Type |Description|
    |---------|-----|-----------|
    |identifier|String|The identifier of a property, event, or service.|
    |operateType|String|The operation type. Supported values include:    -   coilStatus
    -   inputStatus
    -   holdingRegister
    -   inputRegister
|
    |registerAddress|String|The register address.|
    |originalDataType|Object|The original data type.|
    |type|String|Supported values include:int16, uint16, int32, uint32, int64, uint64, float, double, string, and customized data.

|
    |specs|Object|The description.|
    |registerCount|Integer|The number of data in the register.|
    |swap16|Integer|Swaps the first 8 bits and the last 8 bits of the 16-bit data in the register.     -   0: false
    -   1: true
|
    |reverseRegister|Integer|Swaps the bits of the original 32-bit data.     -   0: false
    -   1: true
|
    |scaling|Integer|The zoom factor.|
    |pollingTime|Integer|The data collection interval.|
    |trigger|Integer|The data report method.     -   1: report at a specific time
    -   2: report when changes are detected
|

-   When the protocol is OPC UA, the extended product information of sub-devices is as the following:

    ```
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      },
      "services": [
        {
          "outputData": "",
          "identifier": "AngleSelfAdaption",
          "inputData": [
            {
              "identifier": "test01",
              "index": 0
            }
          ],
          "displayName": "test01"
        }
      ],
      "properties": [
        {
          "identifier": "identifier",
          "displayName": "test02"
        },
        {
          "identifier": "identifier_01",
          "displayName": "identifier_01"
        }
      ],
      "events": [
        {
          "outputData": [
            {
              "identifier": "test01",
              "index": 0
            }
          ],
          "identifier": "event1",
          "displayName": "abc"
        }
      ]
    }
    ```

    Parameter description

    |Parameter|Type |Description|
    |---------|-----|-----------|
    |services|Object|The service.|
    |properties|The object.|The property.|
    |The events.|Object|The event.|
    |outputData|Object|The output parameter, such as event reporting data and returned result of a service call.|
    |identifier|String|The identifier.|
    |inputData|Object|The input parameter.|
    |index|Integer|The index information.|
    |displayName|String|The name that is displayed.|

-   When the protocol is a custom protocol, the extended product information of sub-devices is as the following:

    ```
    {
      "profile": {
        "productKey": "a1ZlGQv****"
      },
      "properties": [
        {
          "identifier": "test_02",
          "customize": {
            "test_02": 123
          }
        },
        {
          "identifier": "test_01",
          "customize": {
            "test01": 1
          }
        }
      ]
    }
    ```

    Parameter description

    |Parameter|Type|Description|
    |:--------|:---|:----------|
    |productKey|String|The ProductKey of the product, which is the unique identifier issued by IoT Platform to the product.|
    |properties|Object|Information of properties.|
    |identifier|String|Identifier of a property.|
    |customize|Object|Extended information of a property, which is the extended information you added when you were defining the property.|


## serverList description {#section_qck_bws_2fb .section}

Two protocols \(Modbus and OPC UA\) are supported for sub-device channels.

-   When the protocol is Modbus, sub-device channel data is as the following:

    ```
    [
      {
        "baudRate": 1200,
        "protocol": "RTU",
        "byteSize": 8,
        "stopBits": 2,
        "parity": 1,
        "name": "modbus01",
        "serialPort": "0",
        "serverId": "D73251B427****"
      },
      {
        "protocol": "TCP",
        "port": 8000,
        "ip": "192.168.0.1",
        "name": "modbus02",
        "serverId": "586CB066D****"
      }
    ]
    ```

    |Parameter|Type |Description|
    |:--------|:----|:----------|
    |protocol|String|The protocol type. It can be TCP or RTU.|
    |port|Integer|The port number.|
    |ip|String|The IP address.|
    |name|String|The channel name.|
    |serverId|String|The channel ID.|
    |baudRate|Integer|The baud rate.|
    |byteSize|Integer|The number of bytes.|
    |stopBits|Integer|The stop bit.|
    |parity|Integer|The parity bit. Supported values include:    -   E: Even parity check.
    -   O: Odd parity check.
    -   N: No parity check.
|
    |serialPort|String|The serial port number.|

-   When the protocol is OPC UA, sub-device channel data is as the following:

    ```
    {
      "password": "XIJTginONohPEUAyZx****==",
      "secPolicy": "Basic128Rsa15",
      "name": "server_01",
      "secMode": "Sign",
      "userName": "123",
      "serverId": "55A9D276A7E****",
      "url": "tcp:00",
      "timeout": 10
    }
    ```

    Parameter description

    |Parameter|Type|Description|
    |:--------|:---|:----------|
    |password|String|The password that has been encrypted by the AES encryption algorithm. For information about password encryption for OPC UA, see the information at the end of this table.|
    |secPolicy|String|The encryption policy. Supported options include None, Basic128Rsa15, and Basic256.|
    |secMode|String|The encryption mode. Supported options include None, Sign, and SignAndEncrypt.|
    |name|String|The name of a sub-device channel.|
    |userName|String|The user name.|
    |serverId|String|The ID of a sub-device channel.|
    |url|String|The server connection address.|
    |timeout|Integer|The timeout value.|

    Password encryption method for OPC UA

    Use the AES encryption algorithm and 128-bit \(16-byte\) grouping. The default mode is CBC and the default padding is PKCS5Padding. Use deviceSecret of the device as the secret. The encrypted result is encoded in Base64.

    Code example:

    ```
    private static String instance = "AES/CBC/PKCS5Padding";
    
        private static String algorithm = "AES";
    
        private static String charsetName = "utf-8";
        /**
         * Encryption algorithm
         *
         * @param data (Data to be encrypted)
         * @param deviceSecret (The deviceSecret of the device)
         * @return
         */
        public static String aesEncrypt(String data, String deviceSecret) {
            try {
                Cipher cipher = Cipher.getInstance(instance);
                byte[] raw = deviceSecret.getBytes();
                SecretKeySpec key = new SecretKeySpec(raw, algorithm);
                IvParameterSpec ivParameter = new IvParameterSpec(deviceSecret.substring(0, 16).getBytes());
                cipher.init(Cipher.ENCRYPT_MODE, key, ivParameter);
                byte[] encrypted = cipher.doFinal(data.getBytes(charsetName));
    
                return new BASE64Encoder().encode(encrypted);
            } catch (Exception e) {
                e.printStackTrace();
            }
    
            return null;
        }
    
        public static String aesDecrypt(String data, String deviceSecret) {
            try {
                byte[] raw = deviceSecret.getBytes(charsetName);
                byte[] encrypted1 = new BASE64Decoder().decodeBuffer(data);
                SecretKeySpec key = new SecretKeySpec(raw, algorithm);
                Cipher cipher = Cipher.getInstance(instance);
                IvParameterSpec ivParameter = new IvParameterSpec(deviceSecret.substring(0, 16).getBytes());
                cipher.init(Cipher.DECRYPT_MODE, key, ivParameter);
                byte[] originalBytes = cipher.doFinal(encrypted1);
                String originalString = new String(originalBytes, charsetName);
                return originalString;
            } catch (Exception ex) {
                ex.printStackTrace();
            }
    
            return null;
        }
    
        public static void main(String[] args) throws Exception {
            String text = "test123";
            String secret = "testTNmjyWHQzniA8wEkTNmjyWHQtest";
            String data = null;
            data = aesEncrypt(text, secret);
            System.out.println(data);
            System.out.println(aesDecrypt(data, secret));
        }
    ```


## deviceList description {#section_rdk_bws_2fb .section}

-   When the protocol is Modbus, the connection configuration data of sub-devices is as the following:

    ```
    {
      "deviceConfig": {
        "slaveId": 1,
        "serverId": "D73251B4277742D"
      },
      "productKey": "test02",
      "deviceName": "test01"
    }
    ```

    Parameter description

    |Parameter|Type |Description|
    |---------|-----|-----------|
    |deviceConfig|Object|The device information.|
    |slaveId|Integer|The slave station ID.|
    |serverId|String|The channel ID.|
    |productKey|String|The ProductKey of the sub-device.|
    |deviceName|String|The name of the sub-device.|

-   When the protocol is OPC UA, the connection configuration data of sub-devices is as the following:

    ```
    {
      "deviceConfig": {
        "displayNamePath": "123",
        "serverId": "44895C63E3FF4013924CEF31519ABE7B"
      },
      "productKey": "test01",
      "deviceName": "test_02"
    }
    ```

    Parameter description

    |Parameter|Type |Description|
    |---------|-----|-----------|
    |deviceConfig|Object|The device connection configuration information.|
    |productKey|String|The ProductKey of the sub-device.|
    |deviceName|String|The name of the sub-device.|
    |displayNamePath|String|The customized name of the channel.|
    |serverId|String|The associated channel ID.|


