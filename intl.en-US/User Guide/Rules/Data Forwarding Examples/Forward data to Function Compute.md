# Forward data to Function Compute {#concept_a1m_ttj_wdb .concept}

Rules engine can forward processed data from IoT Platform to Function Compute \(FC\).

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623653033_en-US.png)

Procedure:

1.  On the Function Compute console, create a service and function.
2.  Create a rule to send data processed on IoT Platform to FC, and then enable the rule.
3.  Send a message to the topic that has rules engine configured.
4.  View the function execution statistics on the Function Compute console, or check whether the configuration result is correct based on specific business logic of the function.

## Procedure {#section_pgq_g5j_wdb .section}

1.  Log on to the Function Compute console. Create a service and function.
    1.  Create a service. Service Name is required. Configure other parameters as required.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623653039_en-US.png)

    2.  After you have created a service, create a function.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623653036_en-US.png)

    3.  Select a function template. A blank template is used as an example.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623653037_en-US.png)

    4.  Set parameters for the function.

        The function is configured to directly display data on the Function Compute console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623663038_en-US.png)

        In the proceeding parameters,

        Service Name: Select the service created in [1.a](#step2).

        Function Name: Specify the name of your function.

        Runtime: Configure the running environment for the function, for example, java8.

        Code Configuration: Upload your code.

        Function Handler: Configure the function entry called to run FC. Set it to com.aliyun.fc.FcDemo::handleRequest.

        Configure other parameters as required. For more information, see configurations in [Function Compute](https://partners-intl.aliyun.com/help/product/50980.htm).

    5.  Verify whether the function runs as intended.

        After you create a function, you can run it on the Function Compute console for verification. FC will display information about function output and requests on the Function Compute console.

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623663040_en-US.png)

2.  Configure rules engine after the function successfully passes the verification.
3.  Before you configure rules engine, follow the instructions in [Create and configure a rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#) to write a SQL script to process the data.

    **Note:** Data in JSON and binary formats can be forwarded to FC.

4.  Click a rule name to go to the Rule Details page.
5.  Select **Data Forwarding** **Add Operation**. On the Add Operation page, configure parameters:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623663034_en-US.png)

    -   Select Operation: Select Function Compute.
    -   Region: Select the region that your need to forward data based on your business requirements. If the region does not have any relevant resources, go to Function Compute Console to create resources.

        **Note:** Data forwarding to FC is supported in regions including China \(Shanghai\), Singapore, and Japan \(Tokyo\).

    -   Service: Select a service based on your region. If there are no services available, click **Create Service**.
    -   Function: Select a function based on your region. If there are no functions available, click **Create Function**.
    -   Authorization: Specify the role granted IoT Platform the permission to operate functions. You need to create a role with permissions to operate functions before you assign the role to rules engine.
6.  Enable the rule. After you run the rule, IoT Platform sends the processed data to FC based on the compiled SQL statements. The Function Compute console directly displays the received data based on the defined function logic.

## Verify the forwarding result {#section_vwt_s5j_wdb .section}

The Function Compute console collects monitored statistics about function execution. Statistics are delayed for five minutes, after which you can view monitored statistics about function execution on the dashboard.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15550623663035_en-US.png)

