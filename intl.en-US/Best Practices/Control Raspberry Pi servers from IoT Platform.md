# Control Raspberry Pi servers from IoT Platform {#concept_f12_wqn_jgb .concept}

Alibaba Cloud IoT Platform allows you to remotely control Raspberry Pi servers without public IP addresses. This topic describes how to control Raspberry Pi servers from IoT Platform, and provides sample development code.

## Background information {#section_ckl_2wh_kgb .section}

Assume that you use Raspberry Pi to build a server at your company or home to run some simple tasks, such as starting a script or downloading files. However, if the Raspberry Pi server does not have a public IP address, you cannot control the server when you are not in the company or at home. If you use other NAT traversal tools to contol the server, disconnection may occur frequently. To resolve these issues, you can combine the IoT Platform [RRPC](../../../../../reseller.en-US/User Guide/RRPC/What is RRPC?.md#) \(remote synchronous process call\) function with [JSch](http://www.jcraft.com/jsch/) to control the Raspberry Pi server from IoT Platform.

## Process {#section_thm_yq3_kgb .section}

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/89951/155359899036953_en-US.png)

Use the following process to control a Raspberry Pi server from IoT Platform:

-   Call the IoT Platform [RRPC method](../../../../../reseller.en-US/Developer Guide (Cloud)/API reference/Communications/RRpc.md#) from your computer to send an SSH command.
-   After receiving the SSH command, IoT Platform sends the SSH command to the Raspberry Pi server by using MQTT.
-   The server runs the SSH command.
-   The server encapsulates the command output into an RRPC response and reports the response to IoT Platform by using MQTT.
-   IoT Platform returns the RRPC response to the computer.

**Note:** The RRPC call timeout period is 5 seconds. If IoT Platform does not receive any reply from the Raspberry Pi server within 5 seconds, a timeout error will be returned. If the command you have sent takes a relatively long time to process, you can ignore the timeout error.

## Download SDKs and demos {#section_tdl_zw3_kgb .section}

To remotely control Raspberry Pi servers, you must first develop the server SDK and device SDK.

-   Install the IoT Platform [server SDK](../../../../../reseller.en-US/Developer Guide (Cloud)/SDK reference/Download SDKs.md#) on the computer. To develop the server, you can use the [server Java SDK demo](https://github.com/aliyun/iotx-api-demo).
-   Install the IoT Platform [device SDK](../../../../../reseller.en-US/Developer Guide (Devices)/Download device SDKs.md#) on the Raspberry Pi server. To develop the device, use the [device Java SDK Demo](http://gaic.alicdn.com/ztms/java-iot-device-sdk-demo-v1130/JavaLinkKitDemo.zip?spm=a2c4g.11186623.2.14.65ba10584QjwPu&file=JavaLinkKitDemo.zip).

The following sections provide examples about how to develop the server SDK and device SDK.

**Note:** In the examples, only simple Linux commands are supported in the code, such as uname, touch, and mv. Complex commands such as file editing are not supported. To implement complex commands, you must write code as needed.

## Develop the device SDK {#section_llg_vy3_kgb .section}

After you have downloaded and installed the device SDK and SDK demo, add project dependencies and the following Java files.

The project can be exported as a JAR package and run on the Raspberry Pi server.

1.  Add dependencies to the pom.xml file.

    ```
    <! -- Device SDK -->
    <dependency>
    	<groupId>com.aliyun.alink.linksdk</groupId>
    	<artifactId>iot-linkkit-java</artifactId>
    	<version>1.1.0</version>
    	<scope>compile</scope>
    </dependency>
    <dependency>
    	<groupId>com.google.code.gson</groupId>
    	<artifactId>gson</artifactId>
    	<version>2.8.1</version>
    	<scope>compile</scope>
    </dependency>
    <dependency>
    	<groupId>com.alibaba</groupId>
    	<artifactId>fastjson</artifactId>
    	<version>1.2.40</version>
    	<scope>compile</scope>
    </dependency>
    
    <! -- SSH client-->
    <! -- https://mvnrepository.com/artifact/com.jcraft/jsch -->
    <dependency>
    	<groupId>com.jcraft</groupId>
    	<artifactId>jsch</artifactId>
    	<version>0.1.55</version>
    </dependency>
    ```

2.  Add the SSHShell.java file that is used to run SSH commands.

    ```
    public class SSHShell {
    
    	private String host;
    
    	private String username;
    
    	private String password;
    
    	private int port;
    
    	private Vector<String> stdout;
    
    	public SSHShell(final String ipAddress, final String username, final String password, final int port) {
    		this.host = ipAddress;
    		this.username = username;
    		this.password = password;
    		this.port = port;
    		this.stdout = new Vector<String>();
    	}
    
    	public int execute(final String command) {
    
    		System.out.println("ssh command: " + command);
    
    		int returnCode = 0;
    		JSch jsch = new JSch();
    		SSHUserInfo userInfo = new SSHUserInfo();
    
    		try {
    			Session session = jsch.getSession(username, host, port);
    			session.setPassword(password);
    			session.setUserInfo(userInfo);
    			session.connect();
    
    			Channel channel = session.openChannel("exec");
    			((ChannelExec) channel).setCommand(command);
    
    			channel.setInputStream(null);
    			BufferedReader input = new BufferedReader(new InputStreamReader(channel.getInputStream()));
    
    			channel.connect();
    
    			String line = null;
    			while ((line = input.readLine()) ! = null) {
    				stdout.add(line);
    			}
    			input.close();
    
    			if (channel.isClosed()) {
    				returnCode = channel.getExitStatus();
    			}
    
    			channel.disconnect();
    			session.disconnect();
    		} catch (JSchException e) {
    			e.printStackTrace();
    		} catch (Exception e) {
    			e.printStackTrace();
    		}
    
    		return returnCode;
    	}
    
    	public Vector<String> getStdout() {
    		return stdout;
    	}
    
    }
    ```

3.  Add the SSHUserInfo.java file that is used to verify the SSH account and password.

    ```
    public class SSHUserInfo implements UserInfo {
    
    	@Override
    	public String getPassphrase() {
    		return null;
    	}
    
    	@Override
    	public String getPassword() {
    		return null;
    	}
    
    	@Override
    	public boolean promptPassphrase(final String arg0) {
    		return false;
    	}
    
    	@Override
    	public boolean promptPassword(final String arg0) {
    		return false;
    	}
    
    	@Override
    	public boolean promptYesNo(final String arg0) {
    		if (arg0.contains("The authenticity of host")) {
    			return true;
    		}
    		return false;
    	}
    
    	@Override
    	public void showMessage(final String arg0) {
    	}
    
    }
    ```

4.  Add the Device.java file that is used to establish an MQTT connection.

    ```
    public class Device {
    
    	/**
    	 * Establish a connection
    	 * 
    	 * @param productKey The product key
    	 * @param deviceName The device name
    	 * @param deviceSecret The device secret
    	 * @throws InterruptedException
    	 */
    	public static void connect(String productKey, String deviceName, String deviceSecret) throws InterruptedException {
    
    		//Initialization parameters
    		LinkKitInitParams params = new LinkKitInitParams();
    
    		//Set MQTT initialization parameters
    		IoTMqttClientConfig config = new IoTMqttClientConfig();
    		config.productKey = productKey;
    		config.deviceName = deviceName;
    		config.deviceSecret = deviceSecret;
    		params.mqttClientConfig = config;
    
    		//Set device certificate information for initialization and import the certificate information
    		DeviceInfo deviceInfo = new DeviceInfo();
    		deviceInfo.productKey = productKey;
    		deviceInfo.deviceName = deviceName;
    		deviceInfo.deviceSecret = deviceSecret;
    		params.deviceInfo = deviceInfo;
    
    		//Initialize the SDK
    		LinkKit.getInstance().init(params, new ILinkKitConnectListener() {
    			public void onError(AError aError) {
    				System.out.println("init failed !! code=" + aError.getCode() + ",msg=" + aError.getMsg() + ",subCode="
    						+ aError.getSubCode() + ",subMsg=" + aError.getSubMsg());
    			}
    
    			public void onInitDone(InitResult initResult) {
    				System.out.println("init success !!") ;
    			}
    		});
    
    		//Perform the subsequent operations only after the initialization is complete. You can increase the sleep time as needed.
    		Thread.sleep(2000);
    	}
    
    	/**
    	 * Publish a message
    	 * 
    	 * @param topic The topic to which the message is published
    	 * @param payload The message payload
    	 */
    	public static void publish(String topic, String payload) {
    		MqttPublishRequest request = new MqttPublishRequest();
    		request.topic = topic;
    		request.payloadObj = payload;
    		request.qos = 0;
    		LinkKit.getInstance().getMqttClient().publish(request, new IConnectSendListener() {
    			@Override
    			public void onResponse(ARequest aRequest, AResponse aResponse) {
    			}
    
    			@Override
    			public void onFailure(ARequest aRequest, AError aError) {
    			}
    		});
    	}
    
    	/**
    	 * Subscribe to a topic
    	 * 
    	 * @param topic The topic of messages to subscribe to
    	 */
    	public static void subscribe(String topic) {
    		MqttSubscribeRequest request = new MqttSubscribeRequest();
    		request.topic = topic;
    		request.isSubscribe = true;
    		LinkKit.getInstance().getMqttClient().subscribe(request, new IConnectSubscribeListener() {
    			@Override
    			public void onSuccess() {
    			}
    
    			@Override
    			public void onFailure(AError aError) {
    			}
    		});
    	}
    
    	/**
    	 * Unsubscribe from a topic
    	 * 
    	 * @param topic The topic of messages to unsubscribe from
    	 */
    	public static void unsubscribe(String topic) {
    		MqttSubscribeRequest request = new MqttSubscribeRequest();
    		request.topic = topic;
    		request.isSubscribe = false;
    		LinkKit.getInstance().getMqttClient().unsubscribe(request, new IConnectUnscribeListener() {
    			@Override
    			public void onSuccess() {
    			}
    
    			@Override
    			public void onFailure(AError aError) {
    			}
    		});
    	}
    
    	/**
    	 * Terminate the connection
    	 */
    	public static void disconnect() {
    		//Perform the deinitialization
    		LinkKit.getInstance().deinit();
    	}
    
    }
    ```

5.  Add the SSHDevice.java file. The SSHDevice.java file includes the main method. You can call the main method to receive RRPC commands, call `SSHShell` to run SSH commands, and return RRPC responses. In the SSHDevice.java file, you must enter the device certificate information, including ProductKey, DeviceName, and DeviceSecret, and the SSH account and password.

    ```
    public class SSHDevice {
    
    	//===================Start to Enter Required Parameters===========================
    	//ProductKey
    	private static String productKey = "";
    	// 
    	private static String deviceName = "";
    	//DeviceSecret
    	private static String deviceSecret = "";
    	//The message communication topic. You can directly use the topic without creating or defining the topic.
    	private static String rrpcTopic = "/sys/" + productKey + "/" + deviceName + "/rrpc/request/+";
    	//The domain name or IP address that you want to access by using SSH
    	private static String host = "127.0.0.1";
    	//The SSH username
    	private static String username = "";
    	//The SSH password
    	private static String password = "";
    	//The SSH port
    	private static int port = 22;
    	//===================End===========================
    
    	public static void main(String[] args) throws InterruptedException {
    
    		//Listen to downstream data
    		registerNotifyListener();
    
    		//Establish the connection
    		Device.connect(productKey, deviceName, deviceSecret);
    
    		//Subscribe to a topic
    		Device.subscribe(rrpcTopic);
    	}
    
    	public static void registerNotifyListener() {
    		LinkKit.getInstance().registerOnNotifyListener(new IConnectNotifyListener() {
    			@Override
    			public boolean shouldHandle(String connectId, String topic) {
    				//Only process messages of the specified topic
    				if (topic.contains("/rrpc/request/")) {
    					return true;
    				} else {
    					return false;
    				}
    			}
    
    			@Override
    			public void onNotify(String connectId, String topic, AMessage aMessage) {
    				//Receive RRPC requests and resturn RRPC responses
    				try {
    					//Run the SSH command
    					String payload = new String((byte[]) aMessage.getData(), "UTF-8");
    					SSHShell sshExecutor = new SSHShell(host, username, password, port);
    					sshExecutor.execute(payload);
    
    					//Obtain the command output
    					StringBuffer sb = new StringBuffer();
    					Vector<String> stdout = sshExecutor.getStdout();
    					for (String str : stdout) {
    						sb.append(str);
    						sb.append("\n");
    					}
    
    					//Return command output to the server
    					String response = topic.replace("/request/", "/response/");
    					Device.publish(response, sb.toString());
    				} catch (UnsupportedEncodingException e) {
    					e.printStackTrace();
    				}
    			}
    
    			@Override
    			public void onConnectStateChange(String connectId, ConnectState connectState) {
    			}
    		});
    	}
    
    }
    ```


## Develop the server SDK {#section_mfd_tbj_kgb .section}

After you have downloaded and installed the server SDK and SDK demo, add project dependencies and the following Java files.

1.  Add dependencies to the pom.xml file.

    ```
    <! -- Server SDK -->
    <dependency>
    	<groupId>com.aliyun</groupId>
    	<artifactId>aliyun-java-sdk-iot</artifactId>
    	<version>6.5.0</version>
    </dependency>
    <dependency>
    	<groupId>com.aliyun</groupId>
    	<artifactId>aliyun-java-sdk-core</artifactId>
    	<version>3.5.1</version>
    </dependency>
    
    <! -- commons-codec -->
    <dependency>
    	<groupId>commons-codec</groupId>
    	<artifactId>commons-codec</artifactId>
    	<version>1.8</version>
    </dependency>
    ```

2.  Add the OpenApiClient.java file that is used to call the IoT Platform API.

    ```
    public class OpenApiClient {
    
    	private static DefaultAcsClient client = null;
    
    	public static DefaultAcsClient getClient(String accessKeyID, String accessKeySecret) {
    
    		if (client ! = null) {
    			return client;
    		}
    
    		try {
    			IClientProfile profile = DefaultProfile.getProfile("cn-shanghai", accessKeyID, accessKeySecret);
    			DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
    			client = new DefaultAcsClient(profile);
    		} catch (Exception e) {
    			System.out.println("create Open API Client failed !! exception:" + e.getMessage());
    		}
    
    		return client;
    	}
    
    }
    ```

3.  Add the SSHCommandSender.java file. The SSHCommandSender.java file contains the main method that is used to send SSH commands and receive responses to SSH commands. In the SSHCommandSender.java file, you must enter your Alibaba Cloud account information including AccessKey ID and AccessKey Secret, and device certificate information including ProductKey and DeviceName, and SSH command.

    ```
    public class SSHCommandSender {
    
    	//===================Start to Enter Required Parameters===========================
    	//AccessKey ID of your Alibaba Cloud account
    	private static String accessKeyID = "";
    	//AccessKey Secret of your Alibaba Cloud account
    	private static String accessKeySecret = "";
    	//ProductKey
    	private static String productKey = "";
    	//DeviceName
    	private static String deviceName = "";
    	//===================End===========================
    
    	public static void main(String[] args) throws ServerException, ClientException, UnsupportedEncodingException {
    
    		//The Linux command
    		String payload = "uname -a";
    
    		//Construct an RRPC request
    		RRpcRequest request = new RRpcRequest();
    		request.setProductKey(productKey);
    		request.setDeviceName(deviceName);
    		request.setRequestBase64Byte(Base64.encodeBase64String(payload.getBytes()));
    		request.setTimeout(5000);
    
    		//Obtain information about the client connected to the Raspberry Pi server
    		DefaultAcsClient client = OpenApiClient.getClient(accessKeyID, accessKeySecret);
    
    		//Initiate an RRPC request
    		RRpcResponse response = (RRpcResponse) client.getAcsResponse(request);
    
    		//Process an RRPC response
    		//"response.getSuccess()" only indicates that the RRPC request has been sent. It does not indicate that the device has received the RRPC request and has returned a response.
    		//Identify whether the message has been received and a response has been returned according to the RrpcCode value. For more information, see https://help.aliyun.com/document_detail/69797.html.
    		if (response ! = null && "SUCCESS".equals(response.getRrpcCode())) {
    			//Output the response
    			System.out.println(new String(Base64.decodeBase64(response.getPayloadBase64Byte()), "UTF-8"));
    		} else {
    			//An error occurred while outputting the response and an RRPC code is displayed.
    			System.out.println(response.getRrpcCode());
    		}
    	}
    
    }
    ```


