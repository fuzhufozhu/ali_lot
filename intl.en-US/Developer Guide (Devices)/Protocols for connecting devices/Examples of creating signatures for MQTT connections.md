# Examples of creating signatures for MQTT connections {#concept_188639 .concept}

This topic provides sample signature code for you to develop your devices, so that your devices can communicate with IoT Platform over MQTT without using the device SDK provided by Alibaba Cloud.

## Description {#section_un2_qyu_9yo .section}

We recommend that you use a device SDK provided by Alibaba Cloud. If a device SDK in any language is used, you do not need to configure your own signature mechanism. To view the SDK download path, access [Download device SDKs](reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#).

If you use other methods to connect your devices to IoT Platform, note the following information:

-   You must take the responsibility to ensure connection stability and maintain the keepalive and reconnection mechanisms for MQTT connections.
-   Alibaba Cloud will not provide related technical support for any possible connection issues.
-   If you want to use IoT Platform features, such as OTA, TSL models, and unique-certificate-per-product authentication, you must compile your own code to implement these features. This may consume a lot of development time and bug fixing time.

## Sample code for signature calculation {#section_t4t_mcd_qa7 .section}

If you do not use the device SDKs provided by Alibaba Cloud IoT Platform, click the following links to access the corresponding sample code pages.

-   [sign\_mqtt.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_mqtt.c): the sample code used to implement the signature function.
-   [sign\_api.h](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_api.h): the sample code that defines the data structure used by the signature function.
-   [sign\_sha256.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_sha256.c): the sample code that defines the algorithm implementations that can be used by the signature function. If you have HMACSHA256 implementation on your own platform, you do not need to compile this code file. However, you must provide the `utils_hmac_sha256()` function that can be called by the APIs defined in sign\_mqtt.c.
-   [sign\_test.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_test.c): the sample code used to test the signature function.

## Description of the signature function {#section_2xk_pmh_r6z .section}

|Function prototype| int32\_t IOT\_Sign\_MQTT\(iotx\_mqtt\_region\_types\_t

 region, iotx\_dev\_meta\_info\_t \*meta,

 iotx\_sign\_mqtt\_t \*signout\);

 |
|Description|Outputs the information required to connect to Alibaba Cloud IoT Platform based on the input information about the IoT device authentication. The connection information contains the domain name, MQTT client ID, MQTT username, and MQTT password. Then, you can provide the information to the MQTT client to connect to Alibaba Cloud IoT Platform.|
|Input parameters|The following input parameters are included: -   region: the Alibaba Cloud endpoints to which the specified device connects.

Sample code:

    ``` {#codeblock_yni_mle_9u5}
typedef enum {
    IOTX_CLOUD_REGION_SHANGHAI,   /* Shanghai */
    IOTX_CLOUD_REGION_SINGAPORE,  /* Singapore */
    IOTX_CLOUD_REGION_JAPAN,      /* Japan */
    IOTX_CLOUD_REGION_USA_WEST,   /* America */
    IOTX_CLOUD_REGION_GERMANY,    /* Germany */
    IOTX_CLOUD_REGION_CUSTOM,     /* Custom setting */
    IOTX_CLOUD_DOMAIN_MAX         /* Maximum number of domain */
} iotx_mqtt_region_types_t;
    ```

-   meta: the identity authentication information of the specified device.

**Note:** The API caller must allocate memory for meta.

Sample code:

    ``` {#codeblock_056_v1s_h9h}
typedef struct _iotx_dev_meta_info {
    char product_key[IOTX_PRODUCT_KEY_LEN + 1];
    char product_secret[IOTX_PRODUCT_SECRET_LEN + 1];
    char device_name[IOTX_DEVICE_NAME_LEN + 1];
    char device_secret[IOTX_DEVICE_SECRET_LEN + 1];
} iotx_dev_meta_info_t;
								
    ```

The parameters in this example are described as follows:

    -   product\_key: the ProductKey of the product to which the device belongs.
    -   product\_secret: the ProductSecret of the product to which the device belongs.
    -   device\_name: the device name.
    -   device\_secret: the device secret.

 |
|Output parameters|signout: the output data, which is used to establish an MQTT connection. Sample code:

 ``` {#codeblock_1si_8r9_qld}
typedef struct {
    char hostname[DEV_SIGN_HOSTNAME_MAXLEN];
    uint16_t port;
    char clientid[DEV_SIGN_CLIENT_ID_MAXLEN];
    char username[DEV_SIGN_USERNAME_MAXLEN];
    char password[DEV_SIGN_PASSWORD_MAXLEN];
} iotx_sign_mqtt_t;
								
```

 The parameters in this example are described as follows:

 -   hostname: the complete domain name of the Alibaba Cloud IoT endpoint.
-   port: the port number of the Alibaba Cloud IoT endpoint.
-   clientid: the client ID that must be specified to establish an MQTT connection.
-   username: the username that must be specified to establish an MQTT connection.
-   password: the password that must be specified to establish an MQTT connection.

 |
|Return values| -   0: indicates that the call is successful.
-   -1: indicates that the call has failed because the input parameters are invalid.

 |

## Example on how to use the signature function {#section_6y8_98h_ayr .section}

In this example, the test code in sign\_test.c is used.

``` {#codeblock_ngf_2ng_las}
#include <stdio.h> 
#include <string.h> 
#include "sign_api.h"  //Defines all data structures that are used in the signature function.

//The following macros are used to define the Alibaba Cloud authentication information of the device: ProductKey, ProductSecret, DeviceName, and DeviceSecret.
//In actual product development, the device authentication information must be encrypted by the device manufacturer and stored in the flash of the device or a file.
//These macros are read and used after the device is powered on.
#define EXAMPLE_PRODUCT_KEY     "a1X2bEn****"
#define EXAMPLE_PRODUCT_SECRET  "7jluWm1zql7b****"
#define EXAMPLE_DEVICE_NAME     "example1"
#define EXAMPLE_DEVICE_SECRET   "ga7XA6KdlEeiPXQPpRbAjOZXwG8y****"

int main(int argc, char *argv[])
{
    iotx_dev_meta_info_t meta_info;
    iotx_sign_mqtt_t sign_mqtt;

    memset(&meta_info, 0, sizeof(iotx_dev_meta_info_t));
    //Use the following code to copy the previously defined device identity information to meta_info.
    memcpy(meta_info.product_key, EXAMPLE_PRODUCT_KEY, strlen(EXAMPLE_PRODUCT_KEY));
    memcpy(meta_info.product_secret, EXAMPLE_PRODUCT_SECRET, strlen(EXAMPLE_PRODUCT_SECRET));
    memcpy(meta_info.device_name, EXAMPLE_DEVICE_NAME, strlen(EXAMPLE_DEVICE_NAME));
    memcpy(meta_info.device_secret, EXAMPLE_DEVICE_SECRET, strlen(EXAMPLE_DEVICE_SECRET));

    //Call the signature function to generate various data that is required to establish an MQTT connection.
    IOT_Sign_MQTT(IOTX_CLOUD_REGION_SHANGHAI, &meta_info, &sign_mqtt);
    
    ...
    
}
			
```

