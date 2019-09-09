# Change the color a light bulb emits by using a device shadow {#task_ig3_d1s_ydb .task}

-   Requirement:

    By default, a light bulb emits white light. During work days, the light bulb is set to emit yellow light when switched on. During a weekend or holiday, it is set to emit blue light when switched on.

-   Analysis:

    The light bulb cannot store the last color state after it is switched off. Using the device shadow, the color state can be stored. When the light bulb is switched on, it reads its shadow information, produces the state that is stored in the shadow, and then reports the current state to IoT Platform to store the current state. Then every time the light bulb is switched on, the light bulb reads its shadow and emits the desired color of light or the last reported color of light.


1.  Log on to the IoT Platform console and create the target product and device.
2.  Configure the device shadow information, as shown in the following demo: 

    ``` {#codeblock_d4l_c5u_1js}
    
    /**
    * Device key and secret information
    */
    private static String deviceName = "********";
    private static String productKey = "******";
    private static String deviceSecret = "**************";
    ```

3.  After the light bulb is switched on, it automatically obtains the shadow information. 
    -   No shadow information is available when the light bulb is switched on for the first time. Therefore, the light bulb emits its default color of light and reports this color to the shadow.

        ``` {#codeblock_hg8_n4d_uhw}
        
        JSONObject errorJsonObj = payloadJsonObj.getJSONObject("content");
        String errorCode = errorJsonObj.getString("errorcode");
        String errorMsg = errorJsonObj.getString("errormessage");
        //If the shadow contains no content, the light bulb reports its current state to the shadow.
        if (RESULT_CODE_NO_SHADOW.equals(errorCode)){
        System.out.println("The shadow contains no content. Therefore, the light bulb emits white light and reports the current state to the shadow.") ;
        // Update the device shadow.
        Map attMap = new HashMap(128);
        attMap.put("color", "white");
        String shadowUpdateMsg = genUpdateShadowMsg(attMap);
        System.out.println("updateShadowMsg: " + shadowUpdateMsg);
        MqttMessage shadowMessage = new MqttMessage(shadowUpdateMsg.getBytes("UTF-8"));
        message.setQos(1);
        sampleClient.publish(shadowUpdateTopic, shadowMessage);
        }else {
        System.out.println("errorCode:" + errorCode);
        System.out.println("errorMsg:" + errorMsg);
        }
        ```

    -   When you switch on the light bulb, the light bulb obtains its shadow information. If the shadow contains a desired state color, the light bulb emits the desired state color and reports that color to the shadow. After this occurs, the desired state in the shadow is cleared. If no desired state is available, the light bulb emits the last reported state color of light.

        ``` {#codeblock_s96_1m2_gzo}
        
        JSONObject shadowJsonObj = JSON.parseObject(message.toString());
        JSONObject payloadJsonObj = shadowJsonObj.getJSONObject("payload");
        shadowVersion = shadowJsonObj.getLong("version");
        LogUtil.print("shadowVersion:" + shadowVersion);
        // Parse the desired and reported state.
        JSONObject stateJsonObj = payloadJsonObj.getJSONObject("state");
        String desiredString = stateJsonObj.getString("desired");
        String reportedString = stateJsonObj.getString("reported");
        LogUtil.print("desiredString:" + desiredString);
        if (desiredString ! = null) {
        //Implement the desired state.
        String color = JSON.parseObject(desiredString).getString("color");
        System.out.println("The color the light bulb will emit will be changed to:" + color);
        //Update the color information into the reported section.
        if (color ! = null){
        String updateShadowReportdMsg = genUpdateShadowReportdMsg(reportedString, color);
        LogUtil.print("updateShadowReportdMsg:" + updateShadowReportdMsg);
        MqttMessage cleanShadowMqttMsg = new MqttMessage(updateShadowReportdMsg.getBytes("UTF-8"));
        message.setQos(1);
        sampleClient.publish(shadowUpdateTopic, cleanShadowMqttMsg);
        LogUtil.print("shadow reported msg update success");
        }
        //Clear the desired state.
        String cleanShadowMsg = genCleanShadowMsg(reportedString);
        LogUtil.print("cleanShadowMsg:" + cleanShadowMsg);
        MqttMessage cleanShadowMqttMsg = new MqttMessage(cleanShadowMsg.getBytes("UTF-8"));
        message.setQos(1);
        sampleClient.publish(shadowUpdateTopic, cleanShadowMqttMsg);
        LogUtil.print("send clean shadow msg done");
        }else {
        //No desired state is available. The light bulb emits the color of light that is specified in the reported section. 
        if (reportedString ! = null){
        System.out.println("The light is switched on, the light bulb emits the color of light that was reported the last time the light was on." + JSON.parseObject(reportedString).getString("color"));
        }
        }
        ```

4.  Verify that the device shadow configuration is successful. 

    -   Switch on the light for the first time and run the device shadow for the first time.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7643/15680213014223_en-US.png)

    -   After the light is switched off, the service changes the desired color. Switch on the light.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7643/15680213014224_en-US.png)

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7643/15680213014225_en-US.png)

    -   Switch off the light, and then switch on the light again.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7643/15680213014226_en-US.png)

    If you want to change the color of light that the light bulb emits, you only need to modify the device shadow content. By using the device shadow, the light bulb that cannot store its states is now able to emit light in different colors.


Complete demo code

``` {#codeblock_qts_ncn_vyq}


/**The method that the device uses to query the shadow information.*/
private static final String SHADOW_METHOD_REPLY = "reply";
/**The method that the service uses to publish messages.*/
private static final String SHADOW_METHOD_CONTROL = "control";
private static final String AUTH_RESULT_SUCCESS_KEY = "code";
private static final String RESULT_CODE_SUCCESS = "200";
private static final String RESULT_CODE_NO_SHADOW = "407";
private static final String RESULT_STATUS_SUCCESS = "success";
/**
* Authenticate the server address, which varies by region.
*/
private static String authUrl = "https://iot-auth.cn-shanghai.aliyuncs.com/auth/devicename";
/**
* Device key and secret information
*/
private static String deviceName = "***";
private static String productKey = "***";
private static String deviceSecret = "***";
/**
* Device shadow topics
*/
private static String shadowAckTopic = "/shadow/get/" + productKey + "/" + deviceName;
private static String shadowUpdateTopic = "/shadow/update/" + productKey + "/" + deviceName;
/**
* Device shadow version number
*/
private static long shadowVersion = 0;
/**
* Generate the JSON-formatted shadow data using the attribute key and value.
*
* @param attributeMap
* @return
*/
private static String genUpdateShadowMsg(Map attributeMap) {
Set attSet = attributeMap.keySet();
Map attMap = new LinkedHashMap();
for (String attKey : attSet) {
attMap.put(attKey, attributeMap.get(attKey));
}
Map reportedMap = new LinkedHashMap();
reportedMap.put("reported", attMap);
Map shadowJsonMap = new LinkedHashMap();
shadowJsonMap.put("method", "update");
shadowJsonMap.put("state", reportedMap);
//Automatically update the shadow version number to a later version.
shadowVersion++;
shadowJsonMap.put("version", shadowVersion);
return JSON.toJSONString(shadowJsonMap);
}
/**
* Clear the JSON-formatted shadow data.
*
* @param reportMsg
* @return
*/
private static String genCleanShadowMsg(String reportMsg) {
Map stateMap = new LinkedHashMap();
if (reportMsg == null || reportMsg.length() == 0) {
stateMap.put("reported", "null");
} else {
JSONObject reportJsonObj = JSON.parseObject(reportMsg);
Set attSet = reportJsonObj.keySet();
Map attMap = new LinkedHashMap();
for (String attKey : attSet) {
attMap.put(attKey, reportJsonObj.getString(attKey));
}
stateMap.put("reported", attMap);
}
stateMap.put("desired", "null");
Map cleanShadowMap = new LinkedHashMap();
cleanShadowMap.put("method", "update");
cleanShadowMap.put("state", stateMap);
shadowVersion++;
cleanShadowMap.put("version", shadowVersion);
return JSON.toJSONString(cleanShadowMap);
}
/**
Update the color to the reported state in the shadow.
*
* @param reportMsg
* @param color 
* @return
*/
private static String genUpdateShadowReportdMsg(String reportMsg, String color) {
Map stateMap = new LinkedHashMap();
Map attMap = new LinkedHashMap();
if (reportMsg ! = null){
JSONObject reportJsonObj = JSON.parseObject(reportMsg);
Set attSet = reportJsonObj.keySet();
for (String attKey : attSet) {
attMap.put(attKey, reportJsonObj.getString(attKey));
}
}
attMap.put("color", color);
stateMap.put("reported", attMap);
Map cleanShadowMap = new LinkedHashMap();
cleanShadowMap.put("method", "update");
cleanShadowMap.put("state", stateMap);
shadowVersion++;
cleanShadowMap.put("version", shadowVersion);
return JSON.toJSONString(cleanShadowMap);
}
/**
* Parse the desired state.
*
* @param message
* @param sampleClient
* @throws Exception
*/
private static void parseDesiredMsg(MqttMessage message, MqttClient sampleClient) throws Exception {
JSONObject shadowJsonObj = JSON.parseObject(message.toString());
JSONObject payloadJsonObj = shadowJsonObj.getJSONObject("payload");
shadowVersion = shadowJsonObj.getLong("version");
LogUtil.print("shadowVersion:" + shadowVersion);
//Parse the desired state.
JSONObject stateJsonObj = payloadJsonObj.getJSONObject("state");
String desiredString = stateJsonObj.getString("desired");
String reportedString = stateJsonObj.getString("reported");
LogUtil.print("desiredString:" + desiredString);
//Clear the shadow information.
if (desiredString ! = null) {
//TODO Implement the desired state.
String color = JSON.parseObject(desiredString).getString("color");
System.out.println("The color the light bulb will emit will be changed to:" + color);
//Update the color information into the reported section.
if (color ! = null){
String updateShadowReportdMsg = genUpdateShadowReportdMsg(reportedString, color);
LogUtil.print("updateShadowReportdMsg:" + updateShadowReportdMsg);
MqttMessage cleanShadowMqttMsg = new MqttMessage(updateShadowReportdMsg.getBytes("UTF-8"));
message.setQos(1);
sampleClient.publish(shadowUpdateTopic, cleanShadowMqttMsg);
LogUtil.print("shadow reported msg update success");
}
//Clear the desired state.
String cleanShadowMsg = genCleanShadowMsg(reportedString);
LogUtil.print("cleanShadowMsg:" + cleanShadowMsg);
MqttMessage cleanShadowMqttMsg = new MqttMessage(cleanShadowMsg.getBytes("UTF-8"));
message.setQos(1);
sampleClient.publish(shadowUpdateTopic, cleanShadowMqttMsg);
LogUtil.print("send clean shadow msg done");
}else {
//No desired state is available. The light bulb emits the color of light that is specified in the reported state. 
if (reportedString ! = null){
System.out.println("The light is switched on, the light bulb emits the color of light that was reported the last time the light was on." + JSON.parseObject(reportedString).getString("color"));
}
}
}
public static void main(String... strings) throws Exception {
/* Device identifier.*/
String clientId = productKey + "&" + deviceName;
Map params = new HashMap(16);
/** The product key of the device. */
params.put("productKey", productKey);
/** The device name. */
params.put("deviceName", deviceName);
params.put("timestamp", "" + System.currentTimeMillis());
params.put("clientId", clientId);
//Signature.
params.put("sign", SignUtil.sign(params, deviceSecret, "hmacMD5"));
// Request resources using MQTT.
params.put("resources", "mqtt");
String result = AbstractAliyunWebUtils.doPost(authUrl, params, 5000, 5000);
LogUtil.print("result=[" + result + "]");
JSONObject mapResult;
try {
mapResult = JSON.parseObject(result);
} catch (Exception e) {
System.out.println("https auth result is invalid json fmt");
return;
}
if (RESULT_CODE_SUCCESS.equals(mapResult.getString(AUTH_RESULT_SUCCESS_KEY))) {
LogUtil.print("The authentication is successful." + mapResult.get("data"));
LogUtil.print("data=[" + mapResult + "]");
} else {
System.err.println("The authentication fails.") ;
throw new RuntimeException(
"The authentication fails:" + mapResult.get("code") + "," + mapResult.get("message"));
}
JSONObject data = (JSONObject)mapResult.get("data");
//TODO The signature returned by the server, which is used for authentication against domain hijacking.
//TODO MQTT server.
String targetServer = "ssl://"
+ data.getJSONObject("resources").getJSONObject("mqtt")
.getString("host")
+ ":" + data.getJSONObject("resources").getJSONObject("mqtt")
.getString("port");
String token = data.getString("iotToken");
String iotId = data.getString("iotId");
//Device ID format:
/* The custom device ID, which can contain digits (0-9), lowercase letters (a-z), and uppercase letters (A-Z). */
String mqttClientId = clientId;
/* The IotId generated after authentication. */
String mqttUsername = iotId;
/* The token obtained after authentication, which is valid for seven days. */
String mqttPassword = token;
System.err.println("mqttclientId=" + mqttClientId);
connectMqtt(targetServer, mqttClientId, mqttUsername, mqttPassword);
}
private static void connectMqtt(String url, String clientId, String mqttUsername,
String mqttPassword) throws Exception {
MemoryPersistence persistence = new MemoryPersistence();
SSLSocketFactory socketFactory = createSSLSocket();
final MqttClient sampleClient = new MqttClient(url, clientId, persistence);
MqttConnectOptions connOpts = new MqttConnectOptions();
/* MQTT 3.1.1 */
connOpts.setMqttVersion(4);
connOpts.setSocketFactory(socketFactory);
//Configure whether to enable automatic reconnection.
connOpts.setAutomaticReconnect(true);
//A value of true indicates that all offline messages will be cleared. Offline messages refer to all messages that were not received when messages were sent with QoS 1 or QoS 2.
connOpts.setCleanSession(false);
connOpts.setUserName(mqttUsername);
connOpts.setPassword(mqttPassword.toCharArray());
connOpts.setKeepAliveInterval(65);
LogUtil.print(clientId + "Connect the device to IoT Platform. IoT Platform access address: " + url);
//sampleClient.setManualAcks(true);//ACK messages used to acknowledge receipt of content will not be returned.
sampleClient.connect(connOpts);
sampleClient.setCallback(new MqttCallback() {
@Override
public void connectionLost(Throwable cause) {
LogUtil.print("The connection fails. Cause:" + cause);
cause.printStackTrace();
}
@Override
public void messageArrived(String topic, MqttMessage message) throws Exception {
LogUtil.print("A message is received from topic [" + topic + "]. Message content:["
+ new String(message.getPayload(), "UTF-8") + "], ");
}
@Override
public void deliveryComplete(IMqttDeliveryToken token) {
// If messages are sent with QoS 0, no response parameters will be returned after authentication using a token.
LogUtil.print("The message is successfully sent. " + ((token == null || token.getResponse() == null) ? "null"
: token.getResponse().getKey()));
}
});
LogUtil.print("The connection is successful:---");
// Subscribe to the device shadow topic.
sampleClient.subscribe(shadowAckTopic, new IMqttMessageListener() {
@Override
public void messageArrived(String topic, MqttMessage message) throws Exception {
LogUtil.print("A message is received:" + message + ",topic=" + topic);
JSONObject shadowJsonObj = JSON.parseObject(message.toString());
String shadowMethod = shadowJsonObj.getString("method");
JSONObject payloadJsonObj = shadowJsonObj.getJSONObject("payload");
/* Use the reply method to check whether the message is successfully parsed.*/
if (SHADOW_METHOD_REPLY.equals(shadowMethod)) {
String status = payloadJsonObj.getString("status");
String stateInfo = payloadJsonObj.getString("state");
if (RESULT_STATUS_SUCCESS.equals(status)) {
if (stateInfo == null) {
System.out.println("update shadow success");
} else {
//Parse the desired state.
parseDesiredMsg(message, sampleClient);
}
} else {
JSONObject errorJsonObj = payloadJsonObj.getJSONObject("content");
String errorCode = errorJsonObj.getString("errorcode");
String errorMsg = errorJsonObj.getString("errormessage");
//If the shadow contains no content, the light bulb reports its current state to the shadow.
if (RESULT_CODE_NO_SHADOW.equals(errorCode)){
System.out.println("The shadow contains no content. Therefore, the light bulb emits white light and reports the current state to the shadow.") ;
// Update the device shadow.
Map attMap = new HashMap(128);
attMap.put("color", "white");
String shadowUpdateMsg = genUpdateShadowMsg(attMap);
System.out.println("updateShadowMsg: " + shadowUpdateMsg);
MqttMessage shadowMessage = new MqttMessage(shadowUpdateMsg.getBytes("UTF-8"));
message.setQos(1);
sampleClient.publish(shadowUpdateTopic, shadowMessage);
}else {
System.out.println("errorCode:" + errorCode);
System.out.println("errorMsg:" + errorMsg);
}
}
}
/* Use the control method to parse the desired state and shadow version. */
else if (SHADOW_METHOD_CONTROL.equals(shadowMethod)) {
parseDesiredMsg(message, sampleClient);
}
}
});
//Get the shadow content and parse the shadow version.
String getShadowInfo = "{\"method\": \"get\"}";
MqttMessage shadowMessage = new MqttMessage(getShadowInfo.getBytes("UTF-8"));
shadowMessage.setQos(1);
sampleClient.publish(shadowUpdateTopic, shadowMessage);
//Wait for retrieval of the shadow version.
Thread.sleep(1000);
}
private static SSLSocketFactory createSSLSocket() throws Exception {
SSLContext context = SSLContext.getInstance("TLSV1.2");
context.init(null, new TrustManager[] {new AliyunIotX509TrustManager()}, null);
SSLSocketFactory socketFactory = context.getSocketFactory();
return socketFactory;
}
```

