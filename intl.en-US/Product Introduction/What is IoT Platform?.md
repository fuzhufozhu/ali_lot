# What is IoT Platform? {#concept_ptd_2x4_tdb .concept}

IoT Platform is a device management platform on Alibaba Cloud that enables developers of IoT applications to implement two-way communications between end devices \(such as sensors, final control elements, embedded devices, and smart household electrical appliances\) and the cloud by creating data channels.

IoT Platform has the following benefits:

## Device Connection {#section_n1t_sss_b2b .section}

IoT platform provides device SDKs to help you connect devices to Alibaba Cloud.

-   Provides various solutions for connecting network equipment that uses 2G/3G/4G, NB-IOT, or LoRa technology, to help streamline the management of devices connected over heterogeneous networks. 
-   Provides device SDKs that support various protocols, such as the MQTT and CoAP protocols. This achieves not only real-time synchronization capabilities by enabling persistent connections, but also energy efficient requirements by enabling transient connections.
-   Supports various open-source programming languages and provides guides for embedding SDKs into different chips using your preferred programming languages. This allows enterprises to connect devices with various chips to IoT Platfrom. 

## Device Communication {#section_pqf_gss_b2b .section}

Devices can use the IoT platform for two-way communication with the cloud through the IoT Hub. The platform enables upload and download channels between devices and the cloud to ensure that two-way communications between the devices and the cloud are smooth and reliable. 

## Device Management {#section_gds_2ss_b2b .section}

IoT Platform manages the entire life cycle of devices, including device registration, function definition, script parsing, online debugging, remote configuration, firmware upgrade, remote maintenance, real-time monitoring, grouping, and device removal.

-   Provides Thing Specification Language to simplify application development.
-   Pushes notifications when a device changes status.
-   Provides data storage capabilities, making it easy to read and write massive amounts of device data in real time.
-   Supports the remote upgrade of devices based on Over-The-Air \(OTA\) technology.
-   Provides a device shadow feature that decouples devices and applications to address scenarios with unstable wireless connections.

## Security {#section_klq_xrs_b2b .section}

A multi-layered security strategy is provided to ensure the security of devices connected to the cloud.

-   Authentication
    -   Chip-level security solutions \(ID²\) and the DeviceSecret mechanism are provided to prevent DeviceSecret being cracked. **Security level: high.**
    -   The Unique Certificate per Device authentication mechanism is provided to prevent devices from being attacked. This mechanism applies to scenarios where pools of device certificates \(consisting of ProductKey, DeviceName, and DeviceSecret\) can be installed into device chips in mass production. **Security level: high.**
    -   The Unique Certificate per Product authentication mechanism is provided to reduce the attack risk of devices. This mechanism applies to scenarios where pools of device certificates \(consisting of ProductKey, DeviceName, and DeviceSecret\) cannot be installed into device chips in mass production. **Security level: medium.**
-   Communication Security
    -   Supports various data channels that use TLS \(for example, MQTT and HTTP\) and DTSL \(for example, CoAP\) protocols to ensure the privacy and integrity of data. This applies to scenarios where hardware resources are sufficient, and devices are not sensitive to power consumption. **Security level: high.**
    -   Supports custom data symmetric encryption channels that use TCP \(for example, MQTT\) and UDP \(for example, CoAP\) protocols. This applies to scenarios where hardware resources are insufficient, and devices are sensitive to power consumption. **Security level: medium.**
    -   Permission management is provided to ensure that communications between the devices and the cloud are secure.
    -   Device-level isolation of communication resources \(such as Topic\) is provided to prevent unauthorized operations on devices.

## SQL parsing and data forwarding using the Rule Engine {#section_mgr_5rs_b2b .section}

IoT Platform can integrate with other Alibaba Cloud services by using the Rule Engine. You can set simple rules to transfer device data to Alibaba Cloud services for data storage and computing.  The Rule Engine has the following features:

-   Establishes M2M communications between devices using rules.
-   Transfers data to Message Queue \(MQ\), ensuring that applications can access device data reliably.
-   Transfers data to Table Store, supporting the integration of data acquisition and structured storage.
-   Transfers data to Function Compute, supporting the integration of data acquisition and event-triggered processing.

