# Create a product category {#task_p4s_ktf_vdb .task}

This chapter provides step by step instructions on how to create a product category and set product properties.

1.   Log on to the [IoT Platform console](http://iot.console.aliyun.com/) using your Alibaba Cloud account. 
2.   Select Products, click **Create Product**. 

    The Create Productdialog box appears, as shown in the following figure  [Figure 1](#fig_pr1_q1g_vdb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12786/1955_en-US.png "Create a product")

3.   Set product properties. A product is a collection of specified devices. The product can be used for device management. 

    Here is an example of the Smart Sprinkler Irrigation product.

    -   Select Edition: select the Pro Edition here.
    -   Product Name: Enter Smart\_Sprinkler\_Irrigationas the product name. You can also customize the product name with a unique identifier and ensure that it is functioning under your account.
    -   Node Type: Select Device here.
    -   Device Type: A standard template with a set of predefined features.

        For example, standard features, such as power usage, voltage, ampere, total power consumption are predefined for smart meters. When you select Smart Meter for the device type, the standard features are automatically created based on the device type. You can modify features in the standard template, or add additional user-defined features to the template.

        If the device type is set to None, no standard features are created, but you will still be able to create user-defined features for your product.

        Here we will set it to None.

    -   Data Format: The format of the uploaded or downloaded device data, you can select Alink JSON or Passthrough/Custom. You can only choose one format to use. The two formats cannot be mixed.
        -   Alink JSON is a data exchange protocol between devices and the cloud. It is provided in JSON format in the IoT Platform Pro Edition for developers.
        -   If you want to customize the serial data format, you can choose Passthrough/Custom, and then convert the user-defined data to  Alink JSON script, and configure the data parsing script in the cloud.

             

    -   Product Description: User-defined, it can be empty.
4.   Click **OK**. 

