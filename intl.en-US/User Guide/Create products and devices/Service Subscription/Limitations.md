# Limitations {#concept_sp2_rpp_1fb .concept}

When you use Service Subscription, the following limitations apply.

|Limitation|Description|
|----------|-----------|
|JDK version|Only JDK 8 is supported.|
|Authentication timeout|Once the connection is established, an authentication request is sent immediately. If the authentication is not successful within 15 seconds, the server will close the connection.|
|Receiving data timeout|After the connection is established, the client sends ping packets regularly to maintain the connection. If the server does not receive ping packets or data from the client in 60 seconds, the server will close the connection.|
|Pushing message timeout|The server pushes again 10 failed messages in bulk each time. If the server does not receive an ACK from the client after 10 seconds, the message push times out.|
|Repush policy for failed messages|The stacked messages \(due to client being offline, slow message consumption, or other reasons\) are repushed every 60 seconds.|
|Message storage time|Messages with QoS 0 are saved for one day, and messages with QoS 1 are saved for seven days.|
|Number of connections|Every account can have up to 20 connections simultaneously.|

