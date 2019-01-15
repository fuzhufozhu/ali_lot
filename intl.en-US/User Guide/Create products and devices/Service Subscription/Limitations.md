# Limitations {#concept_sp2_rpp_1fb .concept}

When you use Service Subscription, the following limitations apply.

|Limitation|Description|
|----------|-----------|
|JDK version|Only JDK 8 is supported.|
|Authentication timeout|Once the connection is established, an authentication request is sent immediately. If the authentication is not successful within 15 seconds, the server will close the connection.|
|Receiving data timeout|After the connection is established, the client sends ping packets regularly to maintain the connection. You can set the interval for sending ping packets on your clients. The default value is 30 seconds. The maximum value is 60 seconds.If no Ping packet or data is sent in 60 seconds, the server will close the connection.

If the client has not received any pong packets in the specified time period, the SDK will close the connection and then try to connect again later. The default interval is 60 seconds.

|
|Pushing message timeout|The server pushes again 10 failed messages in bulk each time. If the server does not receive an ACK from the client after 10 seconds, the message push times out.|
|Repush policy for failed messages|The stacked messages \(due to client being offline, slow message consumption, or other reasons\) are repushed every 60 seconds.|
|Message storage time|Messages with QoS 0 are saved for one day, and messages with QoS 1 are saved for seven days.|
|Number of SDK instances|Each account can enable up to 64 SDK instances.|
|Message limit for each tenant|The maximum number of messages sent each second for a single tenant is 1,000 QPS. If your business requires more, you can open a ticket and make a request.|

