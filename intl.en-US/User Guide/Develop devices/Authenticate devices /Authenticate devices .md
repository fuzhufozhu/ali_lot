# Authenticate devices  {#concept_fqy_pjl_vdb .concept}

To secure devices, IoT Platform provides certificates for devices, including product certificates \(ProductKey and ProductSecret\) and device certificates \(DeviceName and DeviceSecret\). A device certificate is a unique identifier used to authenticate a device. Before a device connects to IoT Hub through a protocol, the device reports the product certificate or the device certificate, depending on the authentication method. The device can connect to IoT Platform only when it passes authentication. IoT Platform supports various authentication methods to meet the requirements of different environments.

IoT Platform supports the following authentication methods:

-   Unique-certificate-per-device authentication: Each device has been installed with its own unique device certificate.
-   Unique-certificate-per-product authentication: All devices under a product have been installed with the same product certificate.
-   Sub-device authentication: This method can be applied to sub-devices that connect to IoT Platform through the gateway.

These methods have their own advantages in terms of accessibility and security. You can choose one according to the security requirements of the device and the actual production conditions. The following table shows the comparison among these methods.

|Item|Unique-certificate-per-device authentication|Unique-certificate-per-product authentication|Sub-device authentication|
|----|--------------------------------------------|---------------------------------------------|-------------------------|
|Information written into the device|ProductKey, DeviceName, and DeviceSecret|ProductKey and ProductSecret|ProductKey|
|Whether to enable authentication in IoT Platform|No. Enabled by default.|Yes. You must enable dynamic register.|Yes. You must enable dynamic register.|
|DeviceName pre-registration|Yes. You need to make sure that the specified DeviceName is unique under a  product.|Yes. You need to make sure that the specified DeviceName is unique under a product.|Yes.|
|Certificate installation requirement|Install a unique device certificate on every device. The safety of every device certificate must be guaranteed.|Install the same product certificate on all devices under a product. Make sure that the product certificate is safely kept.|Install the same product certificate into all sub-devices. The security of the gateway must be guaranteed.|
|Security|High|Medium|Medium|
|Upper limit for registrations|Yes. A product can have a maximum of 500,000 devices.|Yes. A product can have a maximum of 500,000 devices.|Yes. A maximum of 200 sub-devices can be registered with one gateway.|
|Other external reliance|None|None|Security of the gateway.|

