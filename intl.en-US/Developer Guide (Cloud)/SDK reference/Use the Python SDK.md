# Use the Python SDK {#reference_qdh_ywb_zdb .reference}

## Install the Python SDK {#section_acm_vtd_zdb .section}

1.  Install a Python IDE.

    Download the Python installation package from [Official website of Python](https://www.python.org/downloads/) and install Python.

2.  Install pip. Skip this step if pip has already been installed.

    Download the pip installation package from [Official website of pip](https://pip.pypa.io/en/stable/installing/) and install pip.

3.  Install the Python SDK.

    Run the following commands as an administrator to install the Python SDK.

    ```
    sudo pip install aliyun-python-sdk-core
    sudo pip install aliyun-python-sdk-iot
    ```

4.  Run the following commands to import the related IoT Python SDK files to a Python file.

    ```
    from aliyunsdkcore import client
    from aliyunsdkiot.request.v20180120 import RegisterDeviceRequest
    from aliyunsdkiot.request.v20180120 import PubRequest
    ...
    ```


## Initialize the SDK {#section_mcr_dzd_zdb .section}

```
accessKeyId = '<your accessKey>'
 accessKeySecret = '<your accessSecret>'
 clt = client.AcsClient(accessKeyId, accessKeySecret, 'cn-shanghai')
```

accessKeyId is the AccessKeyId of your account, and accessKeySecret is the AccessKeySecret corresponding to the AccessKeyId. You can create and view your AccessKey on the AccessKey Management in the console.

## Initiate a call {#section_xdd_5zd_zdb .section}

In this example, call the Pub API to publish messages to the device.

```
request = PubRequest.PubRequest()
request.set_accept_format('json') # Set the response format. The default is XML.
request.set_ProductKey('productKey')
request.set_TopicFullName('/productKey/deviceName/get') #Full name of the topic to which the messages are sent.
request.set_MessageContent('aGVsbG8gd29ybGQ=') #hello world Base64 String
request.set_Qos(0)
result = clt.do_action_with_exception(request)
print 'result : ' + result
```

