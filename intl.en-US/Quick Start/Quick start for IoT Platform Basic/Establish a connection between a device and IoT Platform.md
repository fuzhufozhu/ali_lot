# Establish a connection between a device and IoT Platform {#task_vr3_rbd_5db .concept}

Alibaba Cloud IoT Platform provides device SDKs that allow devices to connect to the platform. In this section, we use a sample program provided by the platform to simulate a device and develop the SDK for the device to implement communication between the device and IoT Platform.

## Prerequisites {#section_l5k_1h1_vdb .section}

1.  Select your development environment and an SDK for the environment. This example uses the C SDK for Linux.
2.  The following software will be used in the SDK development and compilation environment: `make-4.1`, `git-2.7.4`, `gcc-5.4.0`, `gcov-5.4.0`, `lcov-1.12`, `bash-4.3.48`,`tar-1.28`, and `mingw-5.3.1`. Run the following command to install the software:

    `apt-get install -y build-essential make git gcc`


## Connect the device to IoT Platform {#section_u2y_d22_vdb .section}

1.  Use either of the following methods to download the device SDK:
    -   Use gitclone to clone the source code to the Linux system by running the `git clone https://github.com/aliyun/iotkit-embedded` command.

        After the code is cloned, run the `git branch -r` command to view all branches, and run the git checkout command to access the latest branch.

    -   Download the latest compressed package from the SDK download page, and decompress the package in the Linux environment. For more information, see [Download device SDKs](../../../../intl.en-US/Developer Guide (Devices)/Download device SDKs.md#).
2.  Add the obtained ProductKey, DeviceName, and DeviceSecret of the device to the `iotkit-embedded/sample/mqtt/mqtt-example.c` file.

    ```
    #define PRODUCT_KEY "a1wmrZPO8o9"
    #define DEVICE_NAME "cbgiotkJ4O4WW59ivysa"
    #define DEVICE_SECRET "H3cI7***********************ZeSU"
    ```

3.  Run the following commands to compile the SDK and generate a sample program:

    `make distclean`

    `make`

    The generated sample program is in the `iotkit-embedded/output/release/bin` directory.

    ```
    
    +-- bin
    | +-- coap-example
    | +-- ...
    | +-- mqtt-example
    + -- Include
    | +-- exports
    | | +-- iot_export_coap.h
    | | +-- ...
    | +-Iot_export_shadow.h
    | +-- imports
    | +-Iot_import_coap.h
    | | +-- ...
    | | +-- iot_import_ota.h
    | +-- iot_export.h
    | +-- iot_import.h
    +-- lib
    | + -- Libiot_platform.a
    | +-- libiot_sdk.a
    | +-- libiot_tls.a
    +-- src
    +-- coap-example.c
    +-- http-example.c
    +-- Makefile
    +-- mqtt-example.c
    ```

4.  Run the following commands to execute the sample program. You can see that the device becomes online in the console.

    `cd output/release/bin`

    `./mqtt-example`

5.  After the device becomes online, it starts to send data to the platform.

    The following logs are output for the connection:

    ```
    
    ```
    [inf] iotx_device_info_init(40): device_info created successfully!
    [dbg] iotx_device_info_set(50): start to set device info!
    [dbg] iotx_device_info_set(64): device_info set successfully!
    [dbg] guider_print_dev_guider_info(248): ....................................................
    [dbg] guider_print_dev_guider_info(249): ProductKey : a1wmrZPO8o9 
    [dbg] guider_print_dev_guider_info(250): DeviceName : cbgiotkJ4O4WW59ivysa
    [dbg] guider_print_dev_guider_info(251): DeviceID : H3cI7***********************ZeSU
    [dbg] guider_print_dev_guider_info(253): ....................................................
    [dbg] guider_print_dev_guider_info(254): PartnerID Buf : ,partner_id=example.demo.partner-id
    [dbg] guider_print_dev_guider_info(255): ModuleID Buf : ,module_id=example.demo.module-id
    [dbg] guider_print_dev_guider_info(256): Guider URL :
    [dbg] guider_print_dev_guider_info(258): Guider SecMode : 2 (TLS + Direct)
    [dbg] guider_print_dev_guider_info(260): Guider Timestamp : 2524608000000
    [dbg] guider_print_dev_guider_info(261): ....................................................
    [dbg] guider_print_dev_guider_info(267): ....................................................
    [dbg] guider_print_conn_info(225): -----------------------------------------
    [dbg] guider_print_conn_info(226): Host : 10.125.0.27
    [dbg] guider_print_conn_info(227): Port : 1883
    [dbg] guider_print_conn_info(230): ClientID : gsYfsxQJgeD.DailyEnvDN|securemode=2,timestamp=2524608000000,signmethod=hmacsha1,gw=0,partner_id=example.demo.partner-id,module_id=example.demo.module-id|
    [dbg] guider_print_conn_info(232): TLS PubKey : 0x43c910 ('-----BEGIN CERTI ...')
    [dbg] guider_print_conn_info(235): -----------------------------------------
    [inf] iotx_mc_init(1703): MQTT init success!
    [inf] _ssl_client_init(176): Loading the CA root certificate ...
    cert. version : 3
    serial number : 04:00:00:00:00:01:15:4B:5A:C3:94
    issuer name : C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA
    subject name : C=BE, O=GlobalSign nv-sa, OU=Root CA, CN=GlobalSign Root CA
    issued on : 1998-09-01 12:00:00
    expires on : 2028-01-28 12:00:00
    signed using : RSA with SHA1
    RSA key size : 2048 bis
    basic constraints : CA=true
    key usage : Key Cert Sign, CRL Sign
    [inf] _ssl_parse_crt(144): crt content:451
    [inf] _ssl_client_init(184): ok (0 skipped)
    [inf] _TLSConnectNetwork(346): Connecting /a1wmrZPO8o9.iot-as-mqtt.cn-shanghai.aliyuncs.com/1883...
    [inf] mbedtls_net_connect_timeout(291): setsockopt SO_SNDTIMEO timeout: 10s
    [inf] _TLSConnectNetwork(359): ok
    [inf] _TLSConnectNetwork(364): . Setting up the SSL/TLS structure...
    [inf] _TLSConnectNetwork(374): ok
    [inf] _TLSConnectNetwork(409): Performing the SSL/TLS handshake...
    [inf] _TLSCCnnectNetwork(417): ok
    [inf] _TLSConnectNetwork(421): . Verifying peer X. 509 certificate..
    [inf] _real_confirm(93): certificate verification result: 0x04
    [inf] iotx_mc_connect(2035): mqtt connect success!
    ```
    ```


