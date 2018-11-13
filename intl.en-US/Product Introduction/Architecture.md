# Architecture {#concept_txj_mnp_tdb .concept}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7451/15420941423364_en-US.png)

**IoT Hub**

The IoT Hub helps devices connect to the Alibaba Cloud IoT Platform service. The IoT Hub is considered as a data channel for secure communications between devices and the cloud. The IoT  Hub supports both the Pub/Sub and Revert-RPC communication modes. The Pub/Sub mode is a message routing mode based on topics.

The IoT Hub has the following features:

-   High scalability: supports linear dynamic scaling, and allows one billion devices to be connected simultaneously. 
-   End-to-end encryption: The entire communication link is encrypted with RSA and AES to ensure that the data transmission is secure.
-   Real-time messages: After a data channel is successfully established between the device and the IoT Hub, it becomes a persistent connection to reduce handshake time and to ensure that messages arrive in real time.
-   Data passthrough support: The IoT Hub supports binary format data passthrough to the server. To keep data secure and controllable, the IoT Hub does not store device data.
-   Various communication modes: The IoT Hub supports both the Pub/Sub and Revert-RPC communication modes in order to meet your communication needs in various scenarios.
-   Support for multiple protocols: supports connecting devices to IoT Platform using the CoAP, MQTT, and HTTPS protocols.

**Device management**

IoT Platform offers various features to manage devices. These features manage the lifecycle, device groups, device shadows, firmware upgrade, Thing Special Language \(TSL\), data parsing, online debugging, remote maintenance, and real-time monitoring. Device management in different versions of IoT Platform has different features, see [Specifications](reseller.en-US/Product Introduction/Specifications.md#) for details .

**Rules engine**

When a device communicates with IoT Platform using a topic, you can write an SQL expression to process the data in the topic. You can then configure rules to transfer data to other topics or Alibaba Cloud services. For example:

-   You can transfer and store data to RDS, Table Store, and HiTSDB.
-   You can transfer data to the DataHub and then use StreamCompute for stream computing or MaxCompute for large dataset offline computing. You can also transfer data to Function Compute for event-triggered processing.
-   You can transfer data to Message Queue \(MQ\) for highly reliable data consumption.
-   You can transfer data in one topic to another topic to establish M2M communication between devices.

**Security authentication and authorization policies**

Security is very important to IoT. Alibaba Cloud IoT Platform offers  a multi-layered security strategy to ensure that the connection between devices and the cloud is secure.

-   Iot Platform issues a unique certificate for each device, and each device will use its unique certificate for authentication while connecting to the IoT Hub.
-   IoT Platform provides various device authentication methods for developers to address different security needs and production line requirements.
-   Authorization is provided at the device level. A device can only publish or subscribe to topics associated with that particular device. The server handles data through topics under the Alibaba Cloud account using the AccessKey.

