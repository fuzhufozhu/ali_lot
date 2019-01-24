# Devices receive commands from IoT Platform {#task_k3s_sgg_vdb .task}

You can use applications in the cloud to call the SetDeviceProperty interface to send property setting commands to devices. This article introduces how to configure the device SDK to receive commands from IoT Platform.

1.  Import the SDK dependency into the maven project. 

    The following examples show how to import the IoT Platform Java SDK dependency into the maven project.

    ```
    <! -- https://mvnrepository.com/artifact/com.aliyun/aliyun-java-sdk-iot -->
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-iot</artifactId>
        <version>6.4.0</version>
    </dependency>
    ```

    Import the core module of the SDK.

    ```
    <dependency>
        <groupId>com.aliyun</groupId>
        <artifactId>aliyun-java-sdk-core</artifactId>
        <version>3.5.1</version>
    </dependency>
    ```

2.  Initialize the SDK. 

    The region ID in the endpoint must be the same as the region ID of the device. In the following example, the region ID is cn-shanghai.

    ```
    String accessKey = "<your accessKey>";
    String accessSecret = "<your accessSecret>";
    DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
    IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKey, accessSecret);
    DefaultAcsClient client = new DefaultAcsClient(profile); 
    ```

3.  Call the SetDeviceProperty operation to send a property setting request to a device. In the following example, the value of the property LightSwitch is set to 1. 

    Example:

    ```
    SetDevicePropertyRequest request = new SetDevicePropertyRequest();
    request.setProductKey("a1I1xxxxPf4");
    request.setDeviceName("Light001");
    JSONObject itemJson = new JSONObject();
    itemJson.put("LightSwitch", 1);
    request.setItems(itemJson.toString());
    
    try {
        SetDevicePropertyResponse response = client.getAcsResponse(request);
        System.out.println(response.getRequestId() + ", success: " + response.getSuccess());
    } catch (ClientException e) {
        e.printStackTrace();
    }
    ```

    **Note:** For more information about how to call the SetDeviceProperty operation, see [SetDeviceProperty](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Manage devices/SetDeviceProperty.md#).

4.  If the device has received the request, the log output is as follows: 

    ```
    [inf] iotx_mc_handle_recv_PUBLISH(1617): Downstream Topic: '/sys/a1I1nn8vPf4/Light001/thing/service/property/set'
    [inf] iotx_mc_handle_recv_PUBLISH(1618): Downstream Payload:
    
    < {
    <     "method": "thing.service.property.set",
    <     "id": "200864995",
    <     "params": {
    <         "LightSwitch": 1
    <     },
    <     "version": "1.0.0"
    < }
    
    [dbg] iotx_mc_handle_recv_PUBLISH(1623):         Packet Ident : 00000000
    [dbg] iotx_mc_handle_recv_PUBLISH(1624):         Topic Length : 52
    [dbg] iotx_mc_handle_recv_PUBLISH(1628):           Topic Name : /sys/a1I1nn8vPf4/Light001/thing/service/property/set
    [dbg] iotx_mc_handle_recv_PUBLISH(1631):     Payload Len/Room : 101 / 109
    [dbg] iotx_mc_handle_recv_PUBLISH(1632):       Receive Buflen : 166
    [dbg] iotx_mc_handle_recv_PUBLISH(1643): delivering msg ...
    [dbg] iotx_mc_deliver_message(1344): topic be matched
    [inf] dm_msg_proc_thing_service_property_set(134): thing/service/property/set
    [dbg] dm_msg_request_parse(130): Current Request Message ID: 200864995
    [dbg] dm_msg_request_parse(131): Current Request Message Version: 1.0.0
    [dbg] dm_msg_request_parse(132): Current Request Message Method: thing.service.property.set
    [dbg] dm_msg_request_parse(133): Current Request Message Params: {"LightSwitch":1}
    [dbg] dm_ipc_msg_insert(87): dm msg list size: 0, max size: 50
    [inf] dm_msg_response(262): Send URI: /sys/a1I1nn8vPf4/Light001/thing/service/property/set_reply, Payload: {"id":"200864995","code":200,"data":{}}
    [inf] MQTTPublish(515): Upstream Topic: '/sys/a1I1nn8vPf4/Light001/thing/service/property/set_reply'
    [inf] MQTTPublish(516): Upstream Payload:
    
    > {
    >     "id": "200864995",
    >     "code": 200,
    >     "data": {
    >     }
    > }
    
    [inf] dm_client_publish(121): Publish Result: 0
    [inf] _iotx_linkkit_event_callback(223): Receive Message Type: 15
    [inf] _iotx_linkkit_event_callback(225): Receive Message: {"devid":0,"payload":{"LightSwitch":1}}
    [dbg] _iotx_linkkit_event_callback(403): Current Devid: 0
    [dbg] _iotx_linkkit_event_callback(404): Current Payload: {"LightSwitch":1}
    user_property_set_event_handler. 160: Property Set Received, Devid: 0, Request: {"LightSwitch":1}
    ```


