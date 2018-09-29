# Use the .NET SDK {#reference_d1v_zwb_zdb .reference}

## Install the IoT .NET SDK {#section_h44_ng2_zdb .section}

1.  Install a .NET development environment.

    Recommendations:

    -   Microsoft .NET Framework 4.0 and later.
    -   Visual Studio 2010 and later.
2.  Import IoT .NET SDK core library from the Alibaba Cloud.

    Download the package of the IoT .NET SDK core library from [.NET SDK releases](https://develop.aliyun.com/tools/sdk?spm=5176.doc53095.2.4.zA0LmD#/dotnet), and extract the DLL file from the package.

3.  Add a reference to the core library.
    1.  In the Solution Explorer of Visual Studio, right-click your project, and select **Reference**.
    2.  Click **Add Reference**.
    3.  Click **Browse** in the dialog box.
    4.  Select the DDL file aliyun-net-sdk-Iot.dll, and click **OK**.

## Initialize the SDK {#section_gbs_4j2_zdb .section}

**Note:** The following example uses the China \(Shanghai\) region and the corresponding endpoint. You must specify the region of your IoT Platform and the endpoint of your service to use the SDK.

```
using Aliyun.Acs.Core;
using Aliyun.Acs.Core.Exceptions;
using Aliyun.Acs.Core.Profile;
DefaultProfile.addEndpoint("cn-shanghai", "cn-shanghai", "Iot", "iot.cn-shanghai.aliyuncs.com");
IClientProfile clientProfile = DefaultProfile.GetProfile("cn-shanghai", "<your-access-key-id>", "<your-access-key-secret>");
DefaultAcsClient client = new DefaultAcsClient(clientProfile);
```

Create or view your AccessKeyId and AccessKeySecret on the [AccessKey Management](https://ak-console.aliyun.com) page in the Alibaba Cloud console.

## Initiate a call {#section_xsr_yj2_zdb .section}

In this example, call the Pub API to send messages to a topic.

```
PubRequest request = new PubRequest();
request.ProductKey = "<productKey>";
request.TopicFullName = "/" + request.ProductKey + "/<deviceName>/get";
byte[] payload = Encoding.Default.GetBytes("Hello World.") ;
String payloadStr = Convert.ToBase64String(payload);
request.MessageContent = payloadStr;
request.Qos = 0;
try
{
   PubResponse response = client.GetAcsResponse(request);
   Console.WriteLine("publish message result: " + response.Success);
   Console.WriteLine(response.ErrorMessage);
}
catch (ServerException e)
{
   Console.WriteLine(e.ErrorCode);
   Console.WriteLine(e.ErrorMessage);
}
catch (ClientException e)
{
   Console.WriteLine(e.ErrorCode);
   Console.WriteLine(e.ErrorMessage);
}
```

