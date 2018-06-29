# MQTT standard {#concept_jfq_vjw_vdb .concept}

## Supported versions {#section_mly_bkw_vdb .section}

The Alibaba Cloud IoT Platform currently supports MQTT-based connections. Both MQTT versions 3.1 and 3.1.1 are supported. For more information about these protocols, see [MQTT 3.1.1](http://mqtt.org/) and [MQTT 3.1](http://public.dhe.ibm.com/software/dw/webservices/ws-mqtt/mqtt-v3r1.html).

## Comparisons between IoT Platform based MQTT and standard MQTT {#section_gl4_2kw_vdb .section}

-   IoT Platform supports MQTT packets including PUB, SUB, PING, PONG, CONNECT, DISCONNECT, and UNSUB.
-   Supports cleanSession.
-   Does not support will and retain msg.
-   Does not support QoS 2.
-   Supports the RRPC sychronization mode based on native MQTT topics. The server can call the device and obtain a device response result at the same time.

## Security levels {#section_eqt_kkw_vdb .section}

Supports secure connections over protocols such as TLS version 1, TLS version 1.1, and TLS version 1.2.

-   TCP channel plus encrypted chip \(ID² hardware integration\): High security.
-   TCP channel plus symmetric encryption \(uses the device private key for symmetric encryption\): Medium security.
-   TCP \(the data is not encrypted\): Low security.

## Topic standards {#section_z5j_pkw_vdb .section}

After you have created a product, all devices under the product have access to the following topic categories by default:

-   /$\{productKey\}/$\{deviceName\}/update pub
-   /$\{productKey\}/$\{deviceName\}/update/error pub
-   /$\{productKey\}/$\{deviceName\}/get sub
-   /sys/$\{productKey\}/$\{deviceName\}/thing/\# pub&sub
-   /sys/$\{productKey\}/$\{deviceName\}/rrpc/\# pub&sub
-   /broadcast/$\{productKey\}/\# pub&sub

Each topic rule is a topic category. Topic categories are isolated based on devices. Before a device sends a message, replace deviceName with the deviceName of your own device. This prevents the topic from being sent to another device with the same deviceName. The topics are as follows:

-   pub: The permission to submit data to topics.
-   sub: The permission to subscribe to topics.
-   Topic categories with the following format: /$\{productKey\}/$\{deviceName\}/xxx: Can be expanded or customized in the IoT Platform console
-   Topic categories that begin with "/sys": The application protocol communication standards established by the system. User customization is disabled. The topic should comply with the Alibaba Cloud Alink protocol.
-   Topic categories with the following format: /sys/$\{productKey\}/$\{deviceName\}/thing/xxx: The topic category is used by gateway and sub-devices. It is used in gateway scenarios.
-   Topic categories that begin with "/broadcast": Broadcast topics
-   /sys/$\{productKey\}/$\{deviceName\}/rrpc/request/$\{messageId\}: Used to synchronize requests. The server dynamically generates a topic for the message ID.  The device can subscribe to topic categories with wildcard characters.
-   /sys/$\{productKey\}/$\{deviceName\}/rrpc/request/+: After a message is received, a pub message is sent to /sys/$\{productKey\}/$\{deviceName\}/rrpc/response/$\{messageId\}. The server sends a request and receives a response at the same time.

