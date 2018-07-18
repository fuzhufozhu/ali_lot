# Add information in the console {#task_oxm_xdc_5db .task}

When using IoT Platform, add products and devices in the console first. Then, obtain the device authentication and topic information. IoT Platform authenticates devices based on their ProductKey, DeviceName, and DeviceSecret information. It transfers data between devices and the cloud based on topics.

1.   Log on to the IoT Platform console using your Alibaba Cloud account. If this is your first time using this service, you need to make a request to activate it. 
2.   Create a product. A product is a group of devices sharing the same features. Creating a product can help you manage multiple devices at the same time. 
    1.   On the Products page, click **Create Product** and select **Basic Edition**. 
    2.   Enter the Product Name and select Node Type. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7460/1075_en-US.png)

        -   Product Name: Identifies the product. Product names under the same account must be unique. In this example, we use the product model as the product name.
        -   Node Type: Select Device. With this setting, the devices under this product can directly connect to the IoT Hub.
            -   Device: Sub-devices cannot be mounted. The devices can directly connect to the IoT Hub or be mounted to the IoT Hub as a sub-device of the gateway.
            -   Gateway: Sub-devices can be mounted. The gateway has a sub-device management module that maintains the topology of sub-devices. The gateway can synchronize the topology to the cloud.
    3.   On the Notifications page, click **Define Topic Category** to create a new topic category, which transfers information uploaded by the devices. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7460/2116_en-US.png)

        -   Topic Category: Completes the topic suffix. In this example, enter data.
        -   Operation Permission: Select the permission that the device has for the corresponding topic. Select **Publish and Subscribe**. This means that the devices in this product can send messages to topics with a data suffix. They can also subscribe to messages from this topic.
3.   Click **Devices** to enter the Devices page and add devices. 
    1.   Select the product created in the previous step. 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7460/1080_en-US.png)

    2.  Under this product category, click **Add Device** to add a device.![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7460/1082_en-US.png)

 
    3.   Once the devices are added, the following information is shown: ProductKey, DeviceName, and DeviceSecret. You can copy and save this information for device authentication later. The following is an example of the authentication information: 

        ```
        ProductKey: a1wmrZPO8o9
        DeviceName: cbgiotkJ4O4WW59ivysa
        DeviceSecret: H3cI7***********************ZeSU
        
        ```

4.   Click **View** to view the devices you have added. Click Topics on the left side to view the permissions that the devices have to publish or subscribe to messages in different topics. 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7460/2120_en-US.png)


