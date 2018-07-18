# Devices receive commands from the cloud {#task_k3s_sgg_vdb .task}

In this step, the cloud application uses the PUB API to send messages to the topic. By subscribing to the topic, the device receives commands from the cloud.

1.   Modify the device SDK to subscribe to the topic: `/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/get`. 
    1.   Under the `iotkit-embedded/sample/mqtt/mqtt-example.c` directory, subscribe to the topic in the device SDK. The following is example code: 

        ```
        /* Subscribe to a specific topic */
        rc = IOT_MQTT_Subscribe(pclient, TOPIC_GET, IOTX_MQTT_QOS1, _demo_message_arrive, NULL);
        if (rc < 0) {
        IOT_MQTT_Destroy(&pclient);
        EXAMPLE_TRACE("IOT_MQTT_Subscribe() failed, rc = %d", rc);
        rc = -1;
        goto do_exit;
        }
        ```

    2.   The following is an example function that receives and processes data: 

        ```
        static void _demo_message_arrive(void *pcontext, void *pclient, iotx_mqtt_event_msg_pt msg)
        {
        iotx_mqtt_topic_info_pt ptopic_info = (iotx_mqtt_topic_info_pt) msg->msg;
        /* print topic name and topic message */
        EXAMPLE_TRACE("----");
        EXAMPLE_TRACE("Topic: '%.*s' (Length: %d)",
        ptopic_info->topic_len,
        ptopic_info->ptopic,
        ptopic_info->topic_len);
        EXAMPLE_TRACE("Payload: '%.*s' (Length: %d)",
        ptopic_info->payload_len,
        ptopic_info->payload,
        ptopic_info->payload_len);
        EXAMPLE_TRACE("----");
        }
        ```

    3.   Under the `iotkit-embedded` directory, run the make command to compile the SDK. 

        ```
        make distclean
        ```

        ```
        make
        ```

    4.   The sample program generated is under the `iotkit-embedded/output/release/bin` directory. Run the sample program to complete topic subscription. 

        ```
        cd output/release/bin
        ```

        ```
        ./mqtt-example  
        ```

2.   The cloud application issues commands to the devices. 

    The cloud application calls the PUB API to send the data `hello world!` to the topic subscribed by the device. The following is an example of the Java code:

    ```
    import com.aliyuncs.iot.model.v20170420. PubRequest
    PubRequest request = new PubRequest();
    request.setProductKey("a1wmrZPO8o9");
    request.setMessageContent(Base64.encodeBase64String("hello world!".getBytes()));
    request.setTopicFullName("/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/get");
    Request. setqos (0); // Currently supports QoS0 and QoS1
    PubResponse response = client.getAcsResponse(request);
    System.out.println(response.getSuccess());
    System.out.println(response.getErrorMessage());
    ```

    **Note:** 

    -   For a detailed description of how to issue commands from the cloud, see [Platform API](https://partners-intl.aliyun.com/help/doc-detail/30559.htm).
    -   The device has already subscribed to the topic information. Therefore, the platform automatically sends the corresponding message to the device when the cloud application issues this command.
3.   Once the device receives the message from the cloud, the log shows the following: 

    ```
    
    [dbg] iotx_mc_cycle(1277): PUBLISH
    [dbg] iotx_mc_handle_recv_PUBLISH(1091): Packet Ident : 00053506
    [dbg] iotx_mc_handle_recv_PUBLISH(1092): Topic Length : 28
    [dbg] iotx_mc_handle_recv_PUBLISH(1096): Topic Name : /a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/get
    [dbg] iotx_mc_handle_recv_PUBLISH(1099): Payload Len/Room : 19 / 990
    [dbg] iotx_mc_handle_recv_PUBLISH(1100): Receive Buflen : 1024
    [dbg] iotx_mc_handle_recv_PUBLISH(1111): delivering msg ...
    [dbg] iotx_mc_deliver_message(866): topic be matched
    _demo_message_arrive|151 :: ----
    _demo_message_arrive|152 :: packetId: 53506
    _demo_message_arrive|156 :: Topic: '/a1wmrZPO8o9/cbgiotkJ4O4WW59ivysa/get' (Length: 28)
    _demo_message_arrive|160 :: Payload: 'data: hello word!' (Length: 19)
    _demo_message_arrive|161 :: ----
    ```


