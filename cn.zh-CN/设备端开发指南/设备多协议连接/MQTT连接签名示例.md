# MQTT连接签名示例 {#concept_188639 .concept}

若您不使用阿里云提供的设备端SDK，而是使用其他方式，自己进行开发使您的设备使用MQTT协议与物联网平台连接，您可以参见本文提供的签名代码示例进行MQTT连接签名。

## 说明 {#section_un2_qyu_9yo .section}

推荐您使用阿里云提供的设备端SDK。使用阿里云提供的任何一种语言的设备端SDK，则不用您自己配置签名机制。请访问[下载设备端SDK](cn.zh-CN/设备端开发指南/下载设备端SDK.md#)查看阿里云提供的SDK下载路径。

如果您不使用阿里云提供的设备端SDK，而是使用其他方式将您的设备接入物联网平台，需了解：

-   需要您自己保证连接的稳定性、MQTT连接保活和MQTT连接断开重连。
-   不使用设备端SDK连接阿里云物联网平台导致的连接问题，阿里云不负责相关的技术支持。
-   如果您要使用物联网平台提供的OTA、物模型、一型一密等多种功能，需您自己去编写这些功能的实现。这将会耗费较多的开发时间、以及bug修复时间。

## 签名计算代码示例 {#section_t4t_mcd_qa7 .section}

若您不使用阿里云物联网平台设备端SDK，可点击以下链接，访问相关代码示例页面。

-   [sign\_mqtt.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_mqtt.c) : 实现签名函数的代码示例。
-   [sign\_api.h](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_api.h)：定义签名函数用到的数据结构的代码示例。
-   [sign\_sha256.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_sha256.c)：签名函数可能使用的算法实现代码示例。如果您自己的平台上有HMACSHA256的实现，可以不编译本文件，但是需要提供函数`utils_hmac_sha256()`给sign\_mqtt.c中的API调用。
-   [sign\_test.c](https://code.aliyun.com/edward.yangx/public-docs/raw/master/docs/sign_test.c)：用于测试签名函数的代码示例。

## 签名函数API接口说明 {#section_2xk_pmh_r6z .section}

|函数原型| int32\_t IOT\_Sign\_MQTT\(iotx\_mqtt\_region\_types\_t

 region, iotx\_dev\_meta\_info\_t \*meta,

 iotx\_sign\_mqtt\_t \*signout\);

 |
|函数功能|根据输入的IoT设备身份认证信息, 输出连接到阿里云物联网平台时所需要的域名、MQTT ClientID、MQTT Username、MQTT Password。之后，您可以将这些信息提供给MQTT Client用于连接阿里云物联网平台。|
|输入参数|输入参数内容包括： -   region：指定设备需要连接的阿里云站点。

代码示例：

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

-   meta：指定设备的身份认证信息。

**说明：** API调用者需为meta分配内存。

代码示例：

    ``` {#codeblock_056_v1s_h9h}
typedef struct _iotx_dev_meta_info {
    char product_key[IOTX_PRODUCT_KEY_LEN + 1];
    char product_secret[IOTX_PRODUCT_SECRET_LEN + 1];
    char device_name[IOTX_DEVICE_NAME_LEN + 1];
    char device_secret[IOTX_DEVICE_SECRET_LEN + 1];
} iotx_dev_meta_info_t;
								
    ```

其中包含的参数：

    -   product\_key：设备所属产品的ProductKey。
    -   product\_secret：设备所属产品的ProductSecret。
    -   device\_name：设备名称DeviceName。
    -   device\_secret：设备的DeviceSecret。

 |
|输出参数|signout：输出的数据，该数据将用于MQTT连接。 代码示例：

 ``` {#codeblock_1si_8r9_qld}
typedef struct {
    char hostname[DEV_SIGN_HOSTNAME_MAXLEN];
    uint16_t port;
    char clientid[DEV_SIGN_CLIENT_ID_MAXLEN];
    char username[DEV_SIGN_USERNAME_MAXLEN];
    char password[DEV_SIGN_PASSWORD_MAXLEN];
} iotx_sign_mqtt_t;
								
```

 其中包含参数：

 -   hostname：完整的阿里云物联网站点域名。
-   port：阿里云站点的端口号。
-   clientid：MQTT建立连接时需要指定的ClientID。
-   username：MQTT建立连接时需要指定的Username。
-   password：MQTT建立连接时需要指定的Password。

 |
|返回值| -   0：表示成功。
-   -1：表示输入参数非法而失败。

 |

## 签名API使用示例 {#section_6y8_98h_ayr .section}

以下以sign\_test.c中的测试代码为例。

``` {#codeblock_ngf_2ng_las}
#include <stdio.h>
#include <string.h>
#include "sign_api.h"  //包含签名所需的各种数据结构定义

//下面的几个宏用于定义设备的阿里云身份认证信息：ProductKey、ProductSecret、DeviceName、DeviceSecret
//在实际产品开发中，设备的身份认证信息应该是设备厂商将其加密后存放于设备Flash中或者某个文件中，
//设备上电时将其读出后使用
#define EXAMPLE_PRODUCT_KEY     "a1X2bEn****"
#define EXAMPLE_PRODUCT_SECRET  "7jluWm1zql7b****"
#define EXAMPLE_DEVICE_NAME     "example1"
#define EXAMPLE_DEVICE_SECRET   "ga7XA6KdlEeiPXQPpRbAjOZXwG8y****"

int main(int argc, char *argv[])
{
    iotx_dev_meta_info_t meta_info;
    iotx_sign_mqtt_t sign_mqtt;

    memset(&meta_info, 0, sizeof(iotx_dev_meta_info_t));
    //下面的代码是将上面静态定义的设备身份信息赋值给meta_info
    memcpy(meta_info.product_key, EXAMPLE_PRODUCT_KEY, strlen(EXAMPLE_PRODUCT_KEY));
    memcpy(meta_info.product_secret, EXAMPLE_PRODUCT_SECRET, strlen(EXAMPLE_PRODUCT_SECRET));
    memcpy(meta_info.device_name, EXAMPLE_DEVICE_NAME, strlen(EXAMPLE_DEVICE_NAME));
    memcpy(meta_info.device_secret, EXAMPLE_DEVICE_SECRET, strlen(EXAMPLE_DEVICE_SECRET));

    //调用签名函数，生成MQTT连接时需要的各种数据
    IOT_Sign_MQTT(IOTX_CLOUD_REGION_SHANGHAI, &meta_info, &sign_mqtt);
    
    ...
    
}
			
```

