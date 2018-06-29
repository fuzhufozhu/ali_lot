# Define Thing Specification Language models {#task_lws_ktf_vdb .task}

This section describes the procedure of setting features for a product that you have created. 

The system automatically creates standard features in the feature list for the product after you specify the Device Type when creating the product in the IoT Platform Pro.

Take Smart Irrigationas an example, to configure the properties, services, and events, follow these steps:

-   Property: Power Switch
-   Service: Automatic Sprinkler
-   Event: Fault Report

1.   On the Products page, click **View** next to Smart Irrigation. 

    The system displays the Product Information page.

2.   Select Define Feature, and click **Add**. 

    The system displays the Add Feature page, as shown in [Figure 1](#fig_mzw_zgg_vdb).

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2000_en-US.png "Define a property")

3.   Configure the Power Switch property. 

    -   Feature Type: select Property.
    -   Feature Name: enter Power Switch or another name.
    -   Identifier: enter PowerSwitch or another identifier.

        **Note:** The feature names and identifiers under the same product must be unique.

    -   Data Type: select Bool.
    -   Boolean: enter the description of the selected boolean value.

        When the device reports the property of the power switch, value 0 indicates that the power switch is turned off, and value 1 indicates that the power switch is turned on.

    -   Read/Write Type: select Read/Write to enable switch status reporting and remote control.
4.   On the Define Feature page, click **Add** to configure the service of Automatic Sprinkler, as shown in [Figure 2](#fig_ilh_k3g_vdb). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2010_en-US.png "Automatic Sprinkler")

    1.   Set basic information about the service. 

        -   Feature Type: select Service.
        -   Feature Name: enter Automatic Sprinkler or another name.
        -   Identifier: enter AutoSprinkle or another identifier.

            **Note:** The feature names and identifiers under the same product must be unique.

        -   Invoke Method: select Asynchronous.
    2.   Click **Add Parameter** next to Input Parameters. 

        You need to add two parameters for the Automatic Sprinkler service, including Sprinkling Interval and Sprinkling Amount. After you use the Automatic Sprinkler service, specify the sprinkling interval and amount to enable accurate sprinkling.

        The system displays the Add Parameter page, as shown in [Figure 3](#fig_jtf_1kg_vdb).

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2019_en-US.png "Sprinkling interval")

    3.   Set the Sprinkling Interval parameter. 

        -   Parameter Name: enter Sprinkling Interval or another name.
        -   Identifier: enter SprinkleInterval or another identifier.
        -   Data Type: select int32.
        -   Value Range: this is set to 0 to 60. You can also customize your own range.
        -   Unit: select min to start sprinkling at an interval of 0 to 60 minutes.
    4.   Set the Sprinkling Amount parameter, as shown in [Figure 4](#fig_vvy_nlg_vdb). 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2027_en-US.png "Sprinkling amount")

        -   Parameter Name: enter Sprinkling Amount or another name.
        -   Identifier: enter SprinkleVolume or another identifier.
        -   Data Type: select int32.
        -   Value Range: use the range from 0 to 1000 or another range.
        -   Unit: select ml to set the sprinkling amount from 0 to 1,000 ml each time.
    5.   Click **OK**. 

        The service supports response parameters. You can click **Add Parameter** next to Output Parameters to add a response parameter if required.

5.   On the Define Feature page, click **Add** to configure the Fault Report event, as shown in [Figure 5](#fig_bdh_yrh_vdb). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2117_en-US.png "Fault reporting")

    1.   Set the basic parameters for fault reporting. 

        -   Feature Type: select Event.
        -   Feature Name: enter Fault Report.
        -   Identifier: enter ErrorCode or another identifier.
        -   Event Type: select Fault.
    2.   Click **Add Parameter** next to Output Parameters to create a fault report event that contains an error code, as shown in [Figure 6](#fig_tbk_35h_vdb). 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2118_en-US.png "Error code")

        -   Parameter Name: enter Error code or another name.
        -   Identifier: enter ErrorCode.
        -   Data Type: select enum.
        -   Enum item: customize the response parameters that are returned when different faults occur. For example, 0 indicates unusual voltage, 1 indicates that the current is too large, and 2 indicates a network error.
6.   Return to the Define Feature page to view features that have been defined for Automatic Sprinkler, as shown in [Figure 7](#fig_cfd_tvh_vdb). 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/12787/2119_en-US.png "Define a feature")

7.   Click **View TSL** to view TSL that is automatically generated according to the related feature definitions on IoT Platform. 
8.   Click **Export TSL File** to export the TSL file in the model.json format to a local directory. You may use this TSL file when configuring related devices. 

