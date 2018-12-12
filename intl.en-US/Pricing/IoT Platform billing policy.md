# IoT Platform billing policy {#concept_g13_gsv_tdb .concept}

IoT Platform is provided in two editions: IoT Platform Basic and IoT Platform Pro. For IoT Platform Basic, you only pay for your message usage \(counts\). For IoT Platform Pro, you pay for your message usage \(counts\) and device management. The billing method for both editions is Pay-As-You-Go, which means you pay only for what you use with no minimum fees.

## Billing table {#section_yqw_zwz_22b .section}

Billable items for IoT Platform Basic and IoT Platform Pro are listed in the following table. For the billable interfaces, see the appendix in this documentation.

|Item|Description|IoT Platform Basic|IoT Platform Pro|
|----|:----------|:-----------------|----------------|
|Message communication fee\(Device\)

|Messages sent from devices by calling the Pub operation.|√|√|
|Requests sent from devices used to call the RPC interface.|√|√|
|Responses sent from devices used to reply to the Revert-RPC service.|√|√|
|Messages received by devices by calling the Sub operation.|√|√|
|Messages sent and received by calling TSL model related interfaces.|-|√|
|Message communication fee\(Server\)

|Messages sent from the server by calling the Pub or PubBroadcast operation.|√|√|
|Responses that the server sends to devices.|√|√|
|Messages sent from the server by calling the Revert-RPC interface.|√|√|
|Messages sent from the server by calling sub-device related interfaces.|√|√|
|Messages sent from the server by calling device shadow related interfaces.|√|-|
|Messages sent from the server by calling TSL model related interfaces.|-|√|
|Device management fee| Fees are charged for your device management

 You only pay for devices which are active on that day \(living devices\). That is, you only pay the device management fee for devices which have performed message communication on the day.

 |-|√|
|Free tier message categories| -   Connect
-   Connect Ack
-   Disconnect
-   PingReq
-   PingResp
-   Publish Ack
-   Subscribe
-   Subscribe Ack
-   Unsubscribe
-   Unsubscribe Ack
-   The messages forwarded by Rules Engine

**Note:** Messages sent by the rules engine are free of charge. You only pay for fees associated with using associated cloud services if you transfer data to these services.


 |√|√|

## Billing methods for message communication {#section_cpt_mz1_5db .section}

Billable units

-   Billing is calculated according to your message usage \(counts\). See the preceding table to check the billable message categories.
-   The first 1 million \(1,000,000\) messages \(sent and received\) each month are offered as a free quota. The billing cycle starts from the first day of every month at 00:00:00 Beijing Time \(UTC+8\). Unused messages from the free quota do not carry over to the next month.

Message counts

-   One billable message count equals a maximum of 512 bytes.
-   Messages that exceed 512 bytes will be counted as two or more new messages.
-   To calculate the billable message count, divide message size in bytes by 512, and round any resulting value to the next largest integer.

Pricing unit

-   USD 0.8 per 1,000,000 messages

Billing cycles

-   Pay by day. Message counts \(sent or received\) are calculated daily.
-   The value of the payment amount is rounded to two decimal places.

## Billing methods for device management {#section_c41_nyz_22b .section}

Billable units

-   Billing is calculated according to your living device amount on the day. That is, you only pay for the device management fee for devices which have performed message communication on the day.
-   Every Alibaba Cloud account has a free quota of ten living devices each day. If the number of living devices is over ten, the additional devices will be charged for.

Pricing unit

-   USD 0.003 per living device per day

Billing cycles

-   Pay by day. The living device amount is calculated daily.
-   The value of the payment amount is rounded to two decimal places.

**Note:** The above content is for your reference only. The actual fee is subject to the price when you purchase the service.

## Appendix: Billable interfaces {#section_zd2_vgf_h2b .section}

|Interface|IoT Platform Basic|IoT Platform Pro|
|---------|------------------|----------------|
|**Billable interfaces for device**|
|MQTT Publish|IOT\_CMP\_OTA\_Start|√|√|
|IOT\_CMP\_OTA\_Get\_Config|√|√|
|IOT\_CMP\_OTA\_Request\_Image|√|√|
|IOT\_CMP\_Send|√|√|
|IOT\_MQTT\_Publish|√|√|
|IOT\_OTA\_ReportVersion|√|√|
|IOT\_OTA\_RequestImage|√|√|
|IOT\_OTA\_ReportProgress|√|√|
|IOT\_OTA\_GetConfig|√|√|
|IOT\_Shadow\_Construct|√|√|
|IOT\_Shadow\_RegisterAttribute|√|√|
|IOT\_Shadow\_Push|√|√|
|IOT\_Shadow\_Push\_Async|√|√|
|IOT\_Shadow\_Pull|√|√|
|IOT\_Subdevice\_Register|√|√|
|IOT\_Subdevice\_Unregister|√|√|
|IOT\_Subdevice\_Login|√|√|
|IOT\_Subdevice\_Logout|√|√|
|IOT\_Gateway\_Get\_TOPO|√|√|
|IOT\_Gateway\_Get\_Config|√|√|
|IOT\_Gateway\_Publish\_Found\_List|√|√|
|IOT\_Gateway\_Publish|√|√|
|IOT\_Gateway\_RRPC\_Response|√|√|
|linkkit\_answer\_service|-|√|
|linkkit\_invoke\_raw\_service|-|√|
|linkkit\_trigger\_event|-|√|
|linkkit\_invoke\_fota\_service|-|√|
|linkkit\_invoke\_cota\_get\_config|-|√|
|linkkit\_invoke\_cota\_service|-|√|
|CoAP Send|IOT\_CoAP\_SendMessage|√|√|
|HTTP Send|IOT\_HTTP\_SendMessage|√|√|
|These interfaces are free to call, but you may be charged for received messages.|IOT\_CMP\_Register|√|√|
|IOT\_MQTT\_Subscribe|√|√|
|IOT\_Gateway\_Subscribe|√|√|
|IOT\_Gateway\_RRPC\_Register|√|√|
|**Billable interfaces for server**|
|Pub|√|√|
|PubBroadcast|√|√|
|RRpc|√|√|
|DeleteDeviceWhen you delete a sub-device, a message of /sys/\{productKey\}/\{deviceName\}/thing/delete is triggered.

|√|√|
|DisableThingWhen you disable a sub-device, a message of /sys/\{productKey\}/\{deviceName\}/thing/disable is triggered.

|√|√|
|EnableThingWhen you enable a sub-device, a message of /sys/\{productKey\}/\{deviceName\}/thing/enable is triggered.

|√|√|
|NotifyAddThingTopoWhen you notify a gateway to add a topological relationship, a message of /sys/\{productKey\}/\{deviceName\}/thing/topo/add/notify is triggered.

|√|√|
|UpdateDeviceShadow|√|-|
|InvokeThingServiceWhen you run a specified service on a device, a message like /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\} is sent.

|-|√|
|InvokeThingsServiceWhen you run a specified service on multiple devices, messages like /sys/\{productKey\}/\{deviceName\}/thing/service/\{tsl.service.identifier\} are sent.

|-|√|
|SetDevicePropertyWhen you set properties for a specified device, a message of /sys/\{productKey\}/\{deviceName\}/thing/service/property/set are triggered.

|-|√|
|SetDevicesPropertyWhen you set properties for multiple devices, messages of /sys/\{productKey\}/\{deviceName\}/thing/service/property/set are triggered.

|-|√|

