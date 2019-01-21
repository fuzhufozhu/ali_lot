# OTA updates {#concept_sgj_yts_zdb .concept}

Devices in IoT Platform support Over-The-Air \(OTA\) updates. This topic introduces the process of OTA updates, the topics used in OTA updates, and the data formats.

## OTA update process {#section_qqd_r5s_zdb .section}

The process of a firmware OTA update over MQTT protocol is shown as the following figure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14288/154805197911336_en-US.png)

Topics for firmware update:

-   Devices publish messages to the following topic to report firmware versions to IoT Platform.

    ```
    /ota/device/inform/${YourProductKey}/${YourDeviceName}
    ```

-   Devices subscribe to the following topic to receive notifications of firmware updates from IoT Platform.

    ```
    /ota/device/upgrade/${YourProductKey}/${YourDeviceName}
    ```

-   Devices publish messages to the following topic to report the progress of firmware updates to IoT Platform.

    ```
    /ota/device/progress/${YourProductKey}/${YourDeviceName}
    ```


**Note:** 

-   Devices do not periodically send firmware versions to IoT Platform. Instead, they send their firmware versions to IoT Platform only when they start.
-   Even if you have triggered firmware updates for devices in the console, this does not mean that the devices have updated successfully.

    The IoT Platform firmware update system receives update progress reports from the devices when they are updating, \(that is, when the update status of the devices are Updating\).

-   You can view the device firmware version to check whether the OTA update is successful.
-   An offline device cannot receive any update notifications from the OTA server.

    When the device connects to IoT Platform again, the device notifies the OTA server that it is online. After the server receives the notification, it determines whether the device requires an update. If an update is required, the server sends the update message to the device.


## Data format of messages {#section_nm2_m4c_r2b .section}

For OTA development and code examples, see the documentations in [Link Kit SDK](https://help.aliyun.com/product/93051.html).

1.  When devices connects to the OTA service, they report their firmware versions.

    The topic for devices to report firmware versions over MQTT protocol is `/ota/device/inform/${YourProductKey}/${YourDeviceName}`. Message example:

    ```
    
    {
      "id": 1,
      "params": {
        "version": "1.0.0"
      }
    }
    ```

    -   id: The message ID.
    -   version: The current firmware version of the device.
2.  In the IoT Platform console, upload the firmware update file, verify the file using some devices, and then trigger firmware updates for all the devices of a product.

    For more information, see [Firmware update](../../../../../reseller.en-US/User Guide/Monitoring and Maintenance/Firmware update.md#).

3.  When you trigger a batch update in the console, devices of the product will receive the URL of the firmware file.

    Devices subscribe to the topic `/ota/device/upgrade/${YourProductKey}/${YourDeviceName}` to receive update messages. Then, when you initiate firmware update requests to devices, the devices receive the URL of the firmware file from this topic. A message example is as follows:

    ```
    
    {
      "code": "1000",
      "data": {
        "size": 432945,
        "version": "2.0.0",
        "url": "https://iotx-ota-pre.oss-cn-shanghai.aliyuncs.com/nopoll_0.4.4.tar.gz?    Expires=1502955804&OSSAccessKeyId=XXXXXXXXXXXXXXXXXXXX&Signature=XfgJu7P6DWWejstKJgXJEH0qAKU%3D&security-  token=CAISuQJ1q6Ft5B2yfSjIpK6MGsyN1Jx5jo6mVnfBglIPTvlvt5D50Tz2IHtIf3NpAusdsv03nWxT7v4flqFyTINVAEvYZJOPKGrGR0DzDbDasumZsJbo4f%2FMQBqEaXPS2MvVfJ%2BzLrf0ceusbFbpjzJ6xaCAGxypQ12iN%2B%2Fr6%2F5gdc9FcQSkL0B8ZrFsKxBltdUROFbIKP%2BpKWSKuGfLC1dysQcO1wEP4K%2BkkMqH8Uic3h%2Boy%2BgJt8H2PpHhd9NhXuV2WMzn2%2FdtJOiTknxR7ARasaBqhelc4zqA%2FPPlWgAKvkXba7aIoo01fV4jN5JXQfAU8KLO8tRjofHWmojNzBJAAPpYSSy3Rvr7m5efQrrybY1lLO6iZy%2BVio2VSZDxshI5Z3McKARWct06MWV9ABA2TTXXOi40BOxuq%2B3JGoABXC54TOlo7%2F1wTLTsCUqzzeIiXVOK8CfNOkfTucMGHkeYeCdFkm%2FkADhXAnrnGf5a4FbmKMQph2cKsr8y8UfWLC6IzvJsClXTnbJBMeuWIqo5zIynS1pm7gf%2F9N3hVc6%2BEeIk0xfl2tycsUpbL2FoaGk6BAF8hWSWYUXsv59d5Uk%3D",
        "md5": "93230c3bde425a9d7984a594ac55ea1e"
      },
      "id": 1507707025,
      "message": "success"
    }
    ```

    -   size: The size of the file.
    -   md5: The firmware content encrypted by MD5, which is a 32-bit hex string.
    -   url: The URL of the firmware file. The URL is available for 24 hours. The devices must download the firmware file within 24 hours after the URL is generated.
    -   version: The firmware version.
4.  Devices download the firmware from the URL over HTTPS protocol.

    **Note:** The firmware URL will be released within 24 hours.

    During the firmware downloading process, the devices report progress to IoT Platform using the topic `/ota/device/progress/${YourProductKey}/${YourDeviceName}`. Message example:

    ```
    
    {
      "id": 1
      "params": {
        "step":"1", 
        "desc":" xxxxxxxx "
      }   
    }
    ```

    -   id: The message ID.
    -   step:
        -   \[1, 100\]: Values in this range indicate the download progress ratio.
        -   -1: Failed to update.
        -   -2: Failed to download the firmware.
        -   -3: The device authentication failed.
        -   -4: Failed to install the firmware.
    -   desc: Description of the update progress. If an error occurs, the error message is displayed in this parameter.
5.  After devices have been updated, they report the new firmware version using this topic `/ota/device/inform/${YourProductKey}/${YourDeviceName}`. If the reported version is the same as the version defined in the firmware update file, then the update is successful.

    **Note:** The reported version is the only identifier that can determine whether the update is successful. Even if the reported progress is 100%, if the device does not report the new firmware version to IoT Platform, then the update has failed.


## Errors {#section_egm_k4t_xfb .section}

-   Signature error. If the firmware URL received by the device is incomplete or the URL content has been manually modified, the following error occurs:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14288/154805197935716_en-US.png)

-   Failed to download the firmware file. The firmware file URL is expired. The URL is only available for 24 hours after its generation.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/14288/154805198035717_en-US.png)


