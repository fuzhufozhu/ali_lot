# Send configuration data to gateway devices {#concept_n1j_cc4_2fb .concept}

Send extended configuration information of the TSL model and sub-device connection channel configuration that you configured on the cloud to the gateway device.

-   Topic: /sys/\{productKey\}/\{deviceName\}/thing/model/config/push

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
|id|String|The message ID.|
|version|String|The protocol version number. Default value: 1.0.|
|method|String|The method is `thing.model.config.push`.|
|data|Object|Data|
|digest|String|The signature that is used to verify the integrity of the data obtained from url.|
|digestMethod|String|The signature method. The default method is sha256.|
|url|String|The data url that you get from OSS.|

Response message

```
{
  "id":123,
  "code":200,
  "message":"success",
  "data":{
    "digest":"",
    "digestMethod":"",
    "url":""
  }
}
```

url data

```
{
  "modelList": [
    {
      "profile": {
        "productKey": "test01"
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
        "productKey": "test02"
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
      "serverId": "D73251B4277742"
    },
    {
      "protocol": "TCP",
      "port": 8000,
      "ip": "192.168.0.1",
      "name": "modbus02",
      "serverId": "586CB066D6A34"
    },
    {
      "password": "XIJTginONohPEUAyZxLB7Q==",
      "secPolicy": "Basic128Rsa15",
      "name": "server_01",
      "secMode": "Sign",
      "userName": "123",
      "serverId": "55A9D276A7ED470",
      "url": "tcp:00",
      "timeout": 10
    },
    {
      "password": "hAaX5s13gwX2JwyvUkOAfQ==",
      "name": "service_09",
      "secMode": "None",
      "userName": "1234",
      "serverId": "44895C63E3FF401",
      "url": "tcp:00",
      "timeout": 10
    }
  ],
  "deviceList": [
    {
      "deviceConfig": {
        "displayNamePath": "123",
        "serverId": "44895C63E3FF4013924CEF31519ABE7B"
      },
      "productKey": "test01",
      "deviceName": "test_02"
    },
    {
      "deviceConfig": {
        "displayNamePath": "1",
        "serverId": "55A9D276A7ED47"
      },
      "productKey": "test01",
      "deviceName": "test_03"
    },
    {
      "deviceConfig": {
        "slaveId": 1,
        "serverId": "D73251B4277742D"
      },
      "productKey": "test02",
      "deviceName": "test01"
    },
    {
      "deviceConfig": {
        "slaveId": 2,
        "serverId": "586CB066D6A34E"
      },
      "productKey": "test02",
      "deviceName": "test02"
    }
  ],
  "tslList": [
    {
      "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json",
      "profile": {
        "productKey": "test02"
      },
      "services": [
        {
          "outputData": [],
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
              "name": "FeatureTest02"
            }
          ],
          "method": "thing.service.property.set",
          "name": "set",
          "required": true,
          "callType": "async",
          "desc": "Set properties"
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
              "name": "FeatureTest01"
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
              "name": "FeatureTest02"
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
          "desc": "Get properties"
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
          "name": "FeatureTest01",
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
          "name": "FeatureTest02",
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
              "name": "FeatureTest01"
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
              "name": "FeatureTest02"
            }
          ],
          "identifier": "post",
          "method": "thing.event.property.post",
          "name": "post",
          "type": "info",
          "required": true,
          "desc": "Report properties"
        }
      ]
    },
    {
      "schema": "https://iotx-tsl.oss-ap-southeast-1.aliyuncs.com/schema.json",
      "profile": {
        "productKey": "test01"
      },
      "services": [
        {
          "outputData": [],
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
              "name": "FeatureTest01"
            }
          ],
          "method": "thing.service.property.set",
          "name": "set",
          "required": true,
          "callType": "async",
          "desc": "Set properties",
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
              "name": "FeatureTest01"
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
          "desc": "Get properties",
        },
        {
          "outputData": [],
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
              "name": "Parameter1",
            }
          ],
          "method": "thing.service.AngleSelfAdaption",
          "name": "adaptive angle calibration",
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
          "name": "FeatureTest01",
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
              "name": "FeatureTest01"
            }
          ],
          "identifier": "post",
          "method": "thing.event.property.post",
          "name": "post",
          "type": "info",
          "required": true,
          "desc": "Report properties."
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
              "name": "ParameterTest1"
            }
          ],
          "identifier": "event1",
          "method": "thing.event.event1.post",
          "name": "event1",
          "type": "info",
          "required": false
        }
      ]
    }
  ]
}
```

Parameter description

|Parameter|Type |Description|
|---------|-----|-----------|
|modelList|Object|The extended product information of all sub-devices that are mounted to the gateway.|
|serverList|Object|The sub-device channels of the gateway.|
|deviceList|Object|The connection configurations of all sub-devices that are mounted to the gateway.|
|tslList|Object|The TSL of all sub-devices that are mounted to the gateway.|

## modelList description {#section_ctm_tvs_2fb .section}

Currently, the communication protocols Modbus and OPC UA are supported, but the extended information of the two protocols are different.

-   Modbus

    ```
    {
      "profile": {
        "productKey": "test02"
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
    |swap16|Integer|Swaps the first 8 bits and the last 8 bits of the 16-bit data in the register. 0: false; 1: true.|
    |reverseRegister|Integer|Swaps the bits of the original 32-bit data. 0: false; 1: true.|
    |scaling|Integer|The zoom factor.|
    |pollingTime|Integer|The collection interval.|
    |trigger|Integer|The data report method. 1: report at a specific time; 2: report when changes are detected.|

-   OPC UA

    ```
    {
      "profile": {
        "productKey": "test01"
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


## serverList description {#section_qck_bws_2fb .section}

Two protocols \(Modbus and OPC UA\) are supported for channels.

-   Modbus protocol

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
        "serverId": "D73251B4277742"
      },
      {
        "protocol": "TCP",
        "port": 8000,
        "ip": "192.168.0.1",
        "name": "modbus02",
        "serverId": "586CB066D6A34"
      }
    ]
    ```

    |Parameter|Type |Description|
    |---------|-----|-----------|
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

-   OPC UA protocol

    ```
    {
      "password": "XIJTginONohPEUAyZxLB7Q==",
      "secPolicy": "Basic128Rsa15",
      "name": "server_01",
      "secMode": "Sign",
      "userName": "123",
      "serverId": "55A9D276A7ED470",
      "url": "tcp:00",
      "timeout": 10
    }
    ```

    Parameter description

    |Parameter|Type|Description|
    |---------|----|-----------|
    |password|String|The password that has been encrypted by the AES encryption algorithm. For information about password encryption for OPC UA, see the information at the end of this table.|
    |secPolicy|String|The encryption policy. Supported options include None, Basic128Rsa15, and Basic256.|
    |secMode|String|The encryption mode. Supported options include None, Sign, and SignAndEncrypt.|
    |name|String|The server name.|
    |userName|String|The user name.|
    |serverId|String|The server ID.|
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

-   Modbus protocol

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
    |productKey|String|The product ID.|
    |deviceName|String|The name of the device.|

-   OPC UA protocol

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
    |productKey|String|The product ID.|
    |deviceName|String|The name of the device.|
    |displayNamePath|String|The name that is displayed.|
    |serverId|String|The associated channel ID.|


