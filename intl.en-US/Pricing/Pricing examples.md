# Pricing examples {#concept_tsk_xzv_tdb .concept}

## Case 1 {#section_fml_g2b_5db .section}

Your device sends one 0.4 KB message every second to IoT Platform. IoT Platform then delivers the message to five other devices and one application. In this case, five devices and one application will receive this specified message. For this scenario, assume one month equals 30 days.

Your fees are calculated as follows: 

0.4 KB < 0.5 KB, each 0.4 KB message is counted as one message.

Messages sent per month: 1 message/second \* 60 seconds/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 2,592,000 messages.

Messages received per month: 6 messages/second \* 60 seconds/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 15,552,000 messages.

Total messages: 2,592,000 + 15,552,000 = 18,144,000 messages.

The first 1,000,000 messages of each month are offered for free. Therefore, the actual chargeable message count is calculated as follows: 18,144,000 - 1,000,000 =17,144,000.

The fee is USD 0.8 per 1,000,000 messages, and the total fee is calculated as \(USD 0.8/1,000,000\) \* 17,144,000 = USD 13.72.

## Case 2 {#section_xqz_j2b_5db .section}

Your device sends one 0.6 KB message every second to IoT Platform. IoT Platform then stores the message to Table Store.  For this scenario, assume one month equals 30 days.

Your fees are calculated as follows: 

0.5 KB \*2 \> 0.6 KB \>0.5 KB, each 0.6 KB message is counted as two messages.

Messages sent per month: 2 messages/second \* 60 seconds/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 5,184,000 messages.

After you enable the Table Store service, you are charged for fees associated with using Table Store. However, if you use the rules engine to forward messages to Table Store, IoT Platform does not charge for data transfer to Table Store.

The first 1,000,000 messages of each month are offered for free. Therefore, the actual chargeable message count is calculated as follows: 5,184,000 - 1,000,000 = 4,184,000

The fee is USD 0.8 per 1,000,000 messages, and the total fee is calculated as \(USD 0.8/1,000,000\) \* 4,184,000 = USD 3.35.

## Case 3 {#section_r4q_42b_5db .section}

Your application device sends one 0.5 KB message every minute to IoT Platform. IoT Platform then delivers the message to 10 other devices. In this case, 10 devices will receive this specified message and then store the message to  Table Store. 

Your fees are calculated as follows: 

The message is 0.5 KB. Therefore, each message is counted as one message.

Messages sent per month: 1 message/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 86,400 messages

Messages received per month:  10 messages/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 432,000 messages

After you enable the Table Store service,  you are charged for fees associated with using Table Store. However, if you use the rules engine to forward messages to Table Store, IoT Platform does not charge for data transfer to Table Store.

Total messages: 43,200 + 432,000 =  475,200 messages.

No fees apply. The total message count \(sent and received\) does not exceed the free monthly message quota of 1,000,000.

## Case 4 {#section_f35_p2b_5db .section}

Your device sends one 0.6 KB request every minute to IoT Platform by calling the RPC interface. IoT Platform then delivers the message to the server. The server accepts this specified message and replies one 0.4 KB response to IoT Platform. IoT Platform then delivers the response to your device. 

Your fees are calculated as follows:

0.5 KB \*2 \> 0.6 KB \>0.5 KB, each 0.6 KB message is counted as two messages.

Monthly messages: Device \(Requests\) 2 messages/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 86,400 messages;  Server \(Responses\) 1 message/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 43,200 messages.

Total messages: 86,400 + 43,200 = 129,600 messages.

No fees apply. The total message count \(sent and received\) does not exceed the free monthly message quota of 1,000,000.

## Case 5 {#section_g5r_q2b_5db .section}

The server sends one 0.6 KB request every minute to IoT Platform by calling the Revert-RPC interface. IoT Platform then delivers the request to your device. Your device accepts the request and replies one 0.4 KB response to IoT Platform. IoT Platform then delivers the response to the server. 

Your fees are calculated as follows:

0.5 KB \*2 \> 0.6 KB \>0.5 KB, each 0.6 KB message is counted as two messages.

Monthly messages: Server \(Requests\) 2 messages/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 86,400 messages; Device \(Responses\) 1 message/minute \* 60 minutes/hour \* 24 hours/day \* 30 days = 43,200 messages.

Total messages: 86,400 + 43,200 = 129,600 messages.

No fees apply. The total message count \(sent and received\) does not exceed the free monthly message quota of 1,000,000.

