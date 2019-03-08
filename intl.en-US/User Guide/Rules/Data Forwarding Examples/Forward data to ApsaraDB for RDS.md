# Forward data to ApsaraDB for RDS {#concept_lc3_5pt_tdb .concept}

You can configure the rules engine to forward processed data to ApsaraDB for RDS instances in VPCs.

## Limits {#section_y5v_fv3_wdb .section}

-   The ApsaraDB for RDS instances and your IoT Platform service must be in the same region. For example, if your devices are in cn-shanghai region, the data can only be forwarded to RDS instances in the cn-shanghai region.
-   Only RDS instances in VPCs are supported.
-   Only MySQL instances and SQL Server instances are supported.
-   Databases in classic mode and master mode are supported.
-   Binary data cannot be forwarded to ApsaraDB for RDS.

## Preparations {#section_tyl_hv3_wdb .section}

-   Follow the instructions in [Create and configure a rule](reseller.en-US/User Guide/Rules/Data Forwarding/Create and configure a rule.md#) to create a rule and write a SQL script for processing data.
-   Create an ApsaraDB for RDS instance that is in the same region as your devices, and then create a database and a data table.

## Procedure {#section_yfn_vgh_ygb .section}

1.  Click **Add Operation** next to **Data Forwarding**, and then select **Save to RDS**.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7547/15520325602856_en-US.png)

2.  Configure the following parameters as prompted:

    |Parameter|Action|
    |:--------|:-----|
    |Select Operation|Select Save to RDS.|
    |RDS Instance|Select the VPC RDS instance to which IoT Platform data is to be forwarded.|
    |Database|Enter the name of the target database.**Note:** If your database is in the master mode, you need to manually enter the database name.

|
    |Account|Enter the account of the RDS database. The account requires the permissions to read and write data to the database. Otherwise, rules engine cannot write data to the database.**Note:** After rules engine obtains the account, rules engine only writes data that matches this rule to the database.

|
    |Password|Enter the password to log on to the database.|
    |Table Name|Enter the name of the table that will store data from IoT Platform. Rules engine will then write data to this database table.|
    |Key|Enter a field name of the database table. Rules engine will then write data to this field.|
    |Value|Enter a field of the message that you have defined in the data processing SQL statement. This is the value of Key.**Note:** 

    -   Make sure that the data type of the Value field is the same as that of the Key field. Otherwise, the data cannot be written into the database.
    -   You can enter a variable, such as `${deviceName}`, to indicate that device names selected from the topic messages are used as the value.
|
    |Role|Set the role that authorizes IoT Platform to write data to RDS database table.If you have not created such a role, click **Create RAM Role** and create a role in the RAM console.

|

3.  In the Rules page, click the **Start** button corresponding to the rule to start this rule.
4.  Once the configuration is complete, the rules engine will add the following IP addresses to the whitelist to connect to RDS. If one or more of the following IP addresses are not listed, you need to manually add them to the whitelist of the RDS instance:

    -   China \(Shanghai\): 100.104.123.0/24
    -   Singapore: 100.104.106.0/24
    -   US \(Silicon Valley\): 100.104.8.0/24
    -   US \(Virginia\): 100.104.133.64/26
    -   Germany \(Frankfurt\): 100.104.160.192/26
    -   Japan \(Tokyo\): 100.104.160.192/26
    On the Security page of the RDS console, you can set and view the whitelist.

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7547/15520325603010_en-US.png)


