# IoT Platform billing policy {#concept_g13_gsv_tdb .concept}

With IoT Platform pricing, you pay only for what you use with no minimum fees. Billing is calculated according to your message usage \(counts\), including messages sent from devices or applications to IoT Platform, and messages received from IoT Platform to devices or applications.

## Billing table {#section_wk4_rtv_tdb .section}

|Billable message categories|Free tier message categories|
|---------------------------|----------------------------|
| Sent messages: messages that devices or applications send to IOT Platform, including:

 -   Messages sent from devices by calling the Pub operation.
-   Requests sent from devices used to call the RPC interface.
-   Messages received by devices by calling the Sub operation.
-   Responses sent from devices used to reply to the Revert-RPC service.

 | -   Connect
-   Disconnect
-   Ping pong
-   PubAck
-   SubAck
-   Subscribe
-   Unsubscribe
-   The messages delivered by the Rule Engine

**Note:** Messages sent by the Rule Engine are free of charge. You only pay for fees associated with using associated cloud services if you transfer data to these services.


 |
| Received messages: messages that IOT Platform send to devices or applications, including:

 -   Responses that the server sends to devices.
-   Messages sent from the server by calling the Revert-RPC interface.
-   Messages sent from the server by calling the Pub operation.

 |

## Billing methods {#section_cpt_mz1_5db .section}

Billable units

-   Billing is calculated according to your message usage \(counts\). See the preceding table to check the billable message categories.
-   The first 1 million \(1,000,000\) messages \(sent and received\) each month are offered as a free quota.  The billing cycle starts from the first day of every month at 00:00:00 Beijing Time \(UTC+8\). Unused messages from the free quota do not carry over to the next month.

Message counts

-   One billable message count equals a maximum of 512 bytes. 
-   Messages that exceed 512 bytes will be rounded up as two or more new messages.
-   To calculate the billable message count, divide message size in bytes by 512, and round any resulting decimal value to the next largest integer. 

Pricing unit

-   USD 0.8 per 1,000,000 messages

Billing cycles

-   Pay by day. Message counts \(sent or received\) are calculated daily.
-   The value of the payment amount is rounded to two decimal places.

