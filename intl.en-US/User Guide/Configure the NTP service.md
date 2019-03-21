# Configure the NTP service {#concept_s2r_tx3_kgb .concept}

IoT Platform provides the NTP service to resolve the following issues on embedded devices: limited resources, no NTP service available in the system, and inaccurate timestamp.

## How NTP works {#section_jls_xkj_kgb .section}

Based on the NTP protocol, IoT Platform acts as the NTP server. A device sends a message of a specific topic to IoT Platform with the sending time in the message payload. IoT Platform adds the message receiving time and response sending time to the payload of the response packet. After the device receives the response, the device records its local time when it receives the response. All these four time will be used to calculate the time difference between the device and IoT Platform to obtain the exact current time on the device.

**Note:** The NTP service can be used for time calibration only after the device is connected to IoT Platform.

An embedded device, which does not have an accurate time after it is powered, cannot pass the certificate verification during the TLS connection establishment process. If it does not connect to IoT Platform, this issue cannot be resolved by the NTP service of IoT Platform.

## NTP service procedure {#section_btf_by3_kgb .section}

Request topic: `/ext/ntp/${YourProductKey}/${YourDeviceName}/request`

Response topic: `/ext/ntp/${YourProductKey}/${YourDeviceName}/response`

**Note:** ProductKey and DeviceName are part of the device certificate, which can be obtained from the IoT Platform console.

1.  The device subscribes to the topic: `/ext/ntp/${YourProductKey}/${YourDeviceName}/response`.
2.  The device publishes a message with the current timestamp of the device in the payload to the topic `/ext/ntp/${YourProductKey}/${YourDeviceName}/request`. For example:

    ```
    {
        "deviceSendTime":"100"
    }
    ```

    **Note:** The data type of the timestamp, which supports Long and String.

3.  The device receives a response from the NTP server. The payload includes the following information:

    ```
    {
        "deviceSendTime":"100",
        "serverRecvTime":"1010",
        "serverSendTime":"1015",
    }
    ```

4.  The device calculates the current exact Unix time.

    The time when the device receives the message from the server is recorded as $ \{devicerecvtime\}, and the exact time on the device is: `($ {Serverrecvtime} + $ {serversendtime} + $ {devicerecvtime}-$ {devicesendtime})/2`


## Example {#section_iwy_nmj_kgb .section}

In this example, the device time is 100, the server time is 1000, the network delay is 10, and the time spent before the server sends a response for a received request is 5.

| |Device time|Server time|
|:-|:----------|-----------|
|deviceSend|100 \(deviceSendTime\)|1000|
|serverReceive|110|1010（serverRecvTime）|
|serverSend|115|1015（serverSendTime）|
|deviceReceive|125（deviceRecvTime）|1025|

The device calculates the current exact Unix time as \(1010 + 1015 + 125 - 100\)/2 = 1025.

The current server time is 1015. If the device directly uses the timestamp returned from the server, the device will have a time error due to the network delay.

