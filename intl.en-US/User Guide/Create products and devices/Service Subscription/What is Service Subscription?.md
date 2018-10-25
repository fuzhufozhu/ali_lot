# What is Service Subscription? {#concept_d2m_g2v_y2b .concept}

Service clients can directly subscribe to device upload and status messages of products.

Currently, IoT Platform pushes messages through HTTP/2. After you configure the service subscription, IoT Platform pushes messages to your service client through HTTP/2. This means that you can use HTTP/2 SDKs to allow your enterprise server to directly receive messages from IoT Platform. HTTP/2 SDKs provide identity authentication, topic subscription, message sending and message receiving capabilities, and can be used to enable communication between devices and IoT Hub. Specifically, HTTP/2 SDKs allow you to transfer large numbers of messages between IoT Platform and your enterprise server, and support communication between devices and IoT Platform.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/18849/154046055212665_en-US.png)

**Note:** If you are using an old version of IoT Platform and Message Service is being used to transfer messages, you can upgrade your service subscription method to HTTP/2. If you want to continue using Message Service as your message transferring service, IoT Platform will push messages to Message Service, which means your clients must listen to your queues in Message Service in order to receive messages.

