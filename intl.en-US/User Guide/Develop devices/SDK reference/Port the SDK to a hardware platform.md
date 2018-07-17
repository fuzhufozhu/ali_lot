# Port the SDK to a hardware platform {#concept_obp_kmm_b2b .concept}

## Port the C SDK version 2.0 and later {#section_gpg_nmm_b2b .section}

This topic describes the implementation of C SDK version 2.0 and later. Detailed explanations of each layer are included.

For more information, see the [Alibaba Cloud WiKi page](https://github.com/aliyun/iotkit-embedded/wiki). If the platform that you are using is not supported, visit the [Alibaba Cloud GitHub page](https://github.com/aliyun/iotkit-embedded/issues) to submit a ticket.

Overview:

```

+---------------------------+ +---------------------------+
| | | | => The following are generated after build:
| IoT SDK Example Program | | sample/mqtt|coap|ota/*.c |
| | | | output/release/bin/*-example
+---------------------------+ +---------------------------+
| | | | => The APIs provided by the SDK are all declared in
| IoT SDK Interface Layer | | src/sdk-impl/iot_export.h | => The following are generated after build:
| | | |
| IOT_XXX_YYY() APIs | | Has all APIs' prototype | output/release/include/
| | | | iot-sdk/iot_export.h
| | | | iot-sdk/exports/*.h
+---------------------------+ +---------------------------+
| | | | => The APIs provided by the SDK are all implemented in
| | | src/utils: utilities | => The following are generated after build:
| | +---> | src/log: logging |
| | | src/tls: security |
| IoT SDK Core Implements | | src/guider: authenticate | output/release/lib/
| : => | <---+ | src/system: device mgmt | libiot_sdk.a
| : You SHOULD NOT Focus | | src/mqtt: MQTT client |
| : on this unless | | src/coap: CoAP client |
| : you're debugging bugs | | src/http: HTTP client |
| | | src/shadow: device shadow |
| | | src/ota: OTA channel |
| | | |
+---------------------------+ +---------------------------+
| | | | => The SDK only contains sample code. You need to write custom code based on your needs.
| Hardware Abstract Layer | | src/sdk-impl/iot_import.h | => The following are generated after build:
| | | : => |
| HAL_XXX_YYY() APIs | | : HAL_*() declarations | output/release/lib/
| | | | libiot_platform.a
| : You MUST Implement | | src/platform/*/*/*.c | output/release/include/
| : this part for your | | : => | iot-sdk/iot_import.h
| : target device first | | : HAL_*() example impls | iot-sdk/imports/*.h
+---------------------------+ +---------------------------+
```

Compared to version 1.0.1, version 2.0 has enhanced the compiling system and supports flexible iteration and change of functional modules. The architecture of both versions includes the following three layers:

-   The bottom layer is called the hardware abstraction layer \(HAL\), which corresponds to Hardware Abstract Layer.

    This layer abstracts the functions of different embedded target boards. Supported functions include network transmission, TLS or DTLS channel creation, read, and write,  mutex lock, and unlock during memory allocation.

    **Note:** 

    -   You must implement this layer first when you port the SDK to other platforms.
    -   The SDK does not provide a multi-platform implementation of HAL. The HAL implementation in Linux OS \(Ubuntu16.04\) is provided for your reference.
-   The middle layer is called the SDK core implementation layer, which corresponds to IoT SDK Core Implements.

    This layer contains the core implementations of the C SDK. This layer is based on the HAL interface and provides MQTT and CoAP functions, including establishing MQTT or CoAP connections, transmitting MQTT or CoAP packets, checking OTA firmware status, and downloading OTA firmware.

    **Note:** If the HAL layer is implemented correctly, you do not need to make any modifications to this layer when porting the SDK to other platforms.

-   The top layer is called the SDK interface declaration layer, which corresponds to the IoT SDK Interface .

    You can find multiple sample programs in the sample directory on how to use these APIs to implement your business logic. You only need to specify your device information to run these sample programs on a Linux host.


The implementation of each layer is as follows: 

-   Hardware abstraction layer \(HAL\)

    -   In the header file src/sdk-impl/iot\_import.h, declarations of all HAL functions are listed.
    -   In the file src/sdk-impl/imports/iot\_import\_\*.h, dependencies of all features on the HAL interface are listed.
    -   src/sdk-impl/iot\_import.h contains all the subfiles under the imports directory.
    -   In the compiling system of SDK version 2.0 and later, the HAL functions are compiled into output/release/lib/libiot\_platform.a.
    You can run the following command to list all the HAL functions that need to be implemented when porting the SDK.

    `src/sdk-impl$ grep -ro "HAL_[_A-Za-z0-9]*" *|cut -d':' -f2|sort -u|cat -n`

    The result is as follows:

    ```
    
    1 HAL_DTLSSession_create
    2 HAL_DTLSSession_free
    3 HAL_DTLSSession_read
    4 HAL_DTLSSession_write
    5 HAL_Free
    6 HAL_GetModuleID
    7 HAL_GetPartnerID
    8 HAL_Malloc
    9 HAL_MutexCreate
    10 HAL_MutexDestroy
    11 HAL_MutexLock
    12 HAL_MutexUnlock
    13 HAL_Printf
    14 HAL_Random
    15 HAL_SleepMs
    16 HAL_Snprintf
    17 HAL_Srandom
    18 HAL_SSL_Destroy
    19 HAL_SSL_Establish
    20 HAL_SSL_Read
    21 HAL_SSL_Write
    22 HAL_TCP_Destroy
    23 HAL_TCP_Establish
    24 HAL_TCP_Read
    25 HAL_TCP_Write
    26 HAL_UDP_close
    27 HAL_UDP_create
    28 HAL_UDP_read
    29 HAL_UDP_readTimeout
    30 HAL_UDP_write
    31 HAL_UptimeMs
    32 HAL_Vsnprintf
    ```

    For the implementation of these functions, see the example in src/platform. This example has been tested on `Ubuntu16.04` and `Win7` hosts.

    ```
    
    src/platform$ tree
    .
    +-- iot.mk
    +-- os
    | +-- linux
    | | +-- HAL_OS_linux.c
    | | +-- HAL_TCP_linux.c
    | | +-- HAL_UDP_linux.c
    | +-- ubuntu -> linux
    | +-- win7
    | +-- HAL_OS_win7.c
    | +-- HAL_TCP_win7.c
    +-- ssl
    +-- mbedtls
    | +-- HAL_DTLS_mbedtls.c
    | +-- HAL_TLS_mbedtls.c
    +-- openssl
    +-- HAL_TLS_openssl.c
    ```

    Details of the functions are shown in the following table. For more information, see the comments in the code or the [Alibaba Cloud WiKi page](https://github.com/aliyun/iotkit-embedded/wiki).

    |Function|Description|
    |--------|-----------|
    |HAL\_DTLSSession\_create|Initializes a DTLS resource and establishes a DTLS session. Required for CoAP.|
    |HAL\_DTLSSession\_free|Destroys a DTLS session and releases the DTLS resource. Required for CoAP.|
    |HAL\_DTLSSession\_read|Reads data from a DTLS session. Required for CoAP.|
    |HAL\_DTLSSession\_write|Writes data to a DTLS connection. Required for CoAP.|
    |HAL\_Free|Releases heap memory.|
    |HAL\_GetModuleID|This function is only available for our partners and can be implemented as a void function.|
    |HAL\_GetPartnerID|This function is only available for our partners and can be implemented as a void function.|
    |HAL\_Malloc|Requests heap memory.|
    |HAL\_MutexCreate|Creates a mutex for synchronous control. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function.|
    |HAL\_MutexDestroy|Destroys a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function.|
    |HAL\_MutexLock|Locks a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function.|
    |HAL\_MutexUnlock|Unlocks a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function.|
    |HAL\_Printf|Prints logs or debugging information to a serial port or other standard output.|
    |HAL\_Random|Takes an unsigned number as the input, and returns a random unsigned number from zero to the specified unsigned number as the output.|
    |HAL\_SleepMs|Sleep function that makes the current thread sleep for the specified time in milliseconds.|
    |HAL\_Snprintf|Creates a formatted string in a memory buffer area. For more information, see the C99 function snprintf.|
    |HAL\_Srandom|Sets the seed of the random number generator algorithm that is used by HAL\_Random. For more information, see srand.|
    |HAL\_SSL\_Destroy|Destroys a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Establish|Establishes a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Read|Reads data from a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Write|Writes data to a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_TCP\_Destroy|Destroys a TCP connection. Required for MQTT and HTTPS.|
    |HAL\_TCP\_Establish|Establishes a TCP connection and performs DNS resolution.|
    |HAL\_TCP\_Read|Reads data streams from a TCP connection and returns the number of bytes that are read during the specified time.|
    |HAL\_TCP\_Write|Sends data streams to a TCP connection and returns the number of bytes that are sent during the specified time.|
    |HAL\_UDP\_close|Closes a UDP socket.|
    |HAL\_UDP\_create|Creates a UDP socket.|
    |HAL\_UDP\_read|Reads data packets from a UDP socket and returns the number of bytes that are read during the specified time. Blocking read is used.|
    |HAL\_UDP\_readTimeout|Reads data packets from a UDP socket and returns the number of bytes that are read during the specified time.|
    |HAL\_UDP\_write|Sends data packets to a UDP socket and returns the number of bytes that are sent during the specified time. Blocking write is used.|
    |HAL\_UptimeMs|Obtains the time in milliseconds that the device has been powered on.|
    |HAL\_Vsnprintf|String print function that prints a va\_list variable to the specified string.|

    Implementations of these functions are shown in the following table.

    |Function|Description|
    |--------|-----------|
    |**The following functions are required.**|
    |HAL\_Malloc|Requests heap memory.|
    |HAL\_Free|Releases heap memory.|
    |HAL\_SleepMs|Sleep function that makes the current thread sleep for the specified time in milliseconds.|
    |HAL\_Snprintf|Creates a formatted string in a memory buffer area. For more information, see the C99 function snprintf.|
    |HAL\_Printf|Prints logs or debugging information to a serial port or other standard output.|
    |HAL\_Vsnprintf|String print function that prints a va\_list variable to the specified string.|
    |HAL\_UptimeMs|Obtains the time in milliseconds that the device has been powered on.|
    |**The following functions can be implemented as void functions.**|
    |HAL\_GetPartnerID|This function is only available for our partners and can be implemented as a void function.|
    |HAL\_GetModuleID|This function is only available for our partners and can be implemented as a void function.|
    |HAL\_MutexCreate|Creates a mutex for synchronous control. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function. |
    |HAL\_MutexDestroy|Destroys a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function. |
    |HAL\_MutexLock|Locks a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function. |
    |HAL\_MutexUnlock|Unlocks a mutex. Currently, the SDK only supports single-threaded applications. This function can be implemented as a void function. |
    |**When MQTT is not used, the following functions can be implemented as void functions.**|
    |HAL\_SSL\_Destroy|Destroys a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Establish|Establishes a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Read|Reads data from a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_SSL\_Write|Writes data to a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_TCP\_Destroy|Destroys a TLS connection. Required for MQTT and HTTPS.|
    |HAL\_TCP\_Establish|Establishes a TCP connection and performs DNS resolution.|
    |HAL\_TCP\_Read|Reads data streams from a TCP connection and returns the number of bytes that are read during the specified time.|
    |HAL\_TCP\_Write|Sends data streams to a TCP connection and returns the number of bytes that are sent during the specified time.|
    |HAL\_Random|Takes an unsigned number as the input, and returns a random unsigned number from zero to the specified unsigned number as the output.|
    |HAL\_Srandom|Sets the seed of the random number generator algorithm used by HAL\_Random. For more information, see srand.|
    |**When CoAP is not used, the following functions can be implemented as void functions.**|
    |HAL\_DTLSSession\_create|Initializes a DTLS resource and establishes a DTLS session. Required for CoAP.|
    |HAL\_DTLSSession\_free|Destroys a DTLS session and releases the DTLS resource. Required for CoAP.|
    |HAL\_DTLSSession\_read|Reads data from a DTLS connection. Required for CoAP.|
    |HAL\_DTLSSession\_write|Writes data to a DTLS connection. Required for CoAP.|
    |**When ID² is not used, the following functions can be implemented as void functions.**|
    |HAL\_UDP\_close|Closes a UDP socket.|
    |HAL\_UDP\_create|Creates a UDP socket.|
    |HAL\_UDP\_read|Reads data packets from a UDP socket and returns the number of bytes that are read during the specified time. Blocking read is used.|
    |HAL\_UDP\_readTimeout|Reads data packets from a UDP socket and returns the number of bytes that are read during the specified time.|
    |HAL\_UDP\_write|Sends data packets to a UDP socket and returns the number of bytes that are sent during the specified time. Blocking write is used.|

-   SDK core implementation layer

    -   In the header file src/sdk-impl/iot\_export.h, declarations of all functions are listed.
    -   In the file src/sdk-impl/exports/iot\_export\_\*.h, functions of all features are listed.
    -   src/sdk-impl/iot\_export.h contains the subfiles under the exports directory.
    -   In the compiling system of SDK version 2.0 and later, the functions of the core implementation layer are compiled into output/release/lib/libiot\_sdk.a.
-   SDK interface declaration layer and routines

    For more information, see the [Alibaba Cloud GitHub page](https://github.com/aliyun/iotkit-embedded).


## Port the C SDK v1.0.1 to a hardware platform {#section_ost_3nm_b2b .section}

This topic describes how to port the C SDK v1.0.1 to a hardware platform.

The SDK architecture is shown in the following figure:

![](images/6641_en-US.png "SDK architecture")

-   The architecture has been divided into three layers: the hardware abstraction layer, the SDK kernel code, and the application APIs.
-   When you port the SDK to a hardware platform, you need to implement a hardware abstraction interface accordingly.
-   The hardware abstraction layer contains an OS layer, network layer, and SSL layer.

    -   The OS layer mainly contains time and mutex functions, which are stored in directory $\(SDK\_PATH\)/src/platform/os/.
    -   The network layer contains TCP functions in directory $\(SDK\_PATH\)/src/platform/network/.
    -   The SSL layer contains SSL or TLS functions in directory $\(SDK\_PATH\)/src/platform/ssl/.
    The hardware abstraction layer contains data types, OS \(or hardware\) functions, TCP/IP network functions, and SSL \(TLS\) functions. The details are as follows:

    -   Data types

        |No.|Data type|Description|
        |---|---------|-----------|
        |**Custom data types**|
        |1|bool|Boolean|
        |2|int8\_t|8-bit signed integer type|
        |3|uint8\_t|8-bit unsigned integer type|
        |4|int16\_t|16-bit signed integer type|
        |5|uint16\_t|16-bit unsigned integer type|
        |6|int32\_t|32-bit signed integer type|
        |7|uint32\_t|32-bit unsigned integer type|
        |8|int64\_t|64-bit signed integer type|
        |9|uint64\_t|64-bit unsigned integer type|
        |10|uintptr\_t|Unsigned integer type that can be assigned the address of a pointer|
        |11|intptr\_t|Signed integer type that can be assigned the address of a pointer|
        |**Custom keywords**|
        |1|true|Boolean value true. If the target platform does not support this definition, use the macro definition: \# define true \(1\).|
        |2|false|Boolean value false. If the target platform does not support this definition, use the macro definition: \# define false \(0\).|

        -   You can find these definitions in the source file: $\(SDK\_PATH\)/src/platform/os/aliot\_platform\_datatype.h.
        -   Write implementations of these data types in the source file $\(SDK\_PATH\)/src/platform/aliot\_platform\_datatype.h. 
        **Note:** The data types defined in the SDK conform to the C99 standard. If the target platform supports the C99 standard, no modifications need to be made to this code.

    -   OS \(hardware\) functions

        |No.|Function|Description|
        |---|--------|-----------|
        |1|aliot\_platform\_malloc|Allocates memory blocks.|
        |2|aliot\_platform\_free|Releases memory blocks.|
        |3|aliot\_platform\_time\_get\_ms|Obtains the system time in milliseconds.|
        |4|aliot\_platform\_printf|Prints formatted output.|
        |5|aliot\_platform\_ota\_start|Starts OTA update. The OTA feature is not yet supported. You do not need to implement this function.|
        |6|aliot\_platform\_ota\_write|Writes OTA firmware. The OTA feature is not yet supported. You do not need to implement this function.|
        |7|al\_platform\_ota\_finalize|Finishes OTA update. The OTA feature is not yet supported. You do not need to implement this function.|
        |8|aliot\_platform\_msleep|Specifies sleep settings. If the target platform does not have an operating system, implement the function as a delay function.|
        |9|aliot\_platform\_mutex\_create|Creates a mutex. If the target platform does not have an operating system, you do not need to implement this function.|
        |10|aliot\_platform\_mutex\_destroy|Destroys a mutex. If the target platform does not have an operating system, you do not need to implement this function.|
        |11|aliot\_platform\_mutex\_lock|Locks the specified mutex. If the target platform does not have an operating system, you do not need to implement this function.|
        |12|aliot\_platform\_mutex\_unlock|Unlocks the specified mutex. If the target platform does not have an operating system, you do not need to implement this function.|
        |13|aliot\_platform\_module\_get\_pid|This function is only used in specific scenarios. If you do not need this function, implement this function to return null.|

        -   For more information about these functions, see the source file \($\(SDK\_PATH\)/src/platform/os/aliot\_platform\_os.h\).
        -   When implementing these functions, create a folder under the path $\(SDK\_PATH\)/src/platform/os/ to save your implementations. Remember the folder name as it is needed for compilations.
        **Note:** If the target platform does not have an operating system, all application functions cannot be concurrently invoked, including in interrupt service routines.

    -   TCP/IP network functions

        |No.|API name|Description|
        |---|--------|-----------|
        |1|aliot\_platform\_tcp\_establish|Establishes a TCP connection and returns the connection handle.|
        |2|aliot\_platform\_tcp\_destroy|Disconnects a TCP connection.|
        |3|aliot\_platform\_tcp\_write|Writes data to a TCP channel. Do not forget to implement the timeout function.|
        |4|aliot\_platform\_tcp\_read|Reads data from a TCP channel. Do not forget to implement the timeout function.|

        -   For more information about these functions, see the source file $\(SDK\_PATH\)/src/platform/network/aliot\_platform\_network.h.
        -   When implementing these functions, create a folder under the path $\(SDK\_PATH\)/src/platform/network/ to save your implementations.

            **Note:** Remember the folder name as it is needed for compilations.

    -   SSL functions

        |No.|API name|Description|
        |---|--------|-----------|
        |1|aliot\_platform\_ssl\_establish|Establishes an SSL encrypted channel.|
        |2|aliot\_platform\_ssl\_destroy|Releases an SSL channel.|
        |3|aliot\_platform\_ssl\_write|Writes data to an SSL channel. Do not forget to implement the timeout function.|
        |4|aliot\_platform\_ssl\_read|Read data from an SSL channel. Do not forget to implement the timeout function.|

        -   For more information about these functions, see the source file $\(SDK\_PATH\)/src/platform/ssl/aliot\_platform\_ssl.h.
        -   When implementing these functions, create a folder under the path $\(SDK\_PATH\)/src/platform/ssl/ to save your implementations.

            **Note:** Remember the folder name as it is needed for compilations.


