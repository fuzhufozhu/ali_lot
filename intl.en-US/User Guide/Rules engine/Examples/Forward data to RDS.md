# Forward data to RDS {#concept_lc3_5pt_tdb .concept}

You can configure rules engine to forward processed data to RDS instances in VPCs.

## Restrictions {#section_y5v_fv3_wdb .section}

-   Only five nodes support data forwarding to RDS: China East 2, US West 1 \(Silicon Valley\), Asia Pacific SE 1 \(Singapore\), Asia Pacific NE 1 \(Tokyo\), and EU Central 1 \(Frankfurt\).
-   Data can be forwarded only from IoT Platform to RDS within the same region but cannot be forwarded from IoT Platform to RDS in a different region. For example, for an IoT Platform on China East 2, data can only be forwarded to a RDS in China East 2.
-   Only forwarding to RDS instances in VPCs is supported.
-   Only MySQL instances are supported.
-   Supports forwarding to databases in classic and master modes.

## Preparation {#section_tyl_hv3_wdb .section}

Before you configure a forwarding rule, you need to follow the instructions in  [Create and configure a rule](reseller.en-US/User Guide/Rules engine/Create and configure a rule.md#) to write a SQL script and process the data.

## Procedure {#section_ywy_3v3_wdb .section}

1.  Click **Add Operation** next to **Data Forwarding** to open the Add Operation dialog box. Select **Save to RDS**

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7547/2856_en-US.png)

2.  Configure the following parameters as prompted:
    -   Select Operation: Save to RDS.
    -   VPC Instance, MySQL Database: Select the VPC instance and MySQL database in the current region based on your business requirements.

        **Note:** If your database is in the master mode, you need to manually enter the database name.

    -   Account, Password: Enter the account and password to log on to the database. This account should have the permissions to read and write data to the database. Otherwise, rules engine cannot write data to RDS.

        **Note:** After rules engine obtains the account, rules engine only writes data that matches the rule to the database.

    -   Table Name: Enter the name of the table that has been built in the database. Rules engine writes the data to the database table.
    -   Field: Enter the field name of the database. Rules engine writes the processed data to the field.
    -   Value: Enter the value of the field for the database table. You can use the escape character `$`. The format is `${key}`, indicating that the value of key selected from the topic is used as the input value.

        For example, if the SQL statement for rules engine is `SELECT key FROM mytopic`, and RDS has a table that includes a `String` type field with the value `tem`.

        on IoT Platform, enter tem into **Filed**, and $\{key\} \(the JSON field selected from rules engine\) into**Value**.

        **Note:** 

        -   Make sure that the value is set in the correct format: `${}`. Otherwise, a constant is written to the table.
        -   Make sure that the data type of the field is consistent with its value. Otherwise, the data cannot be stored in the database. 
3.  Once the configuration is complete, rules engine will add the following IP addresses to the whitelist to connect to RDS. If the following IP addresses are not listed, manually add them to the whitelist:

    -   China East 2: 100.104.123.0/24
    -   Asia Pacific SE 1 \(Singapore\): 100.104.106.0/24
    -    US West 1 \(Silicon Valley\): 100.104.8.0/24
    -   EU Central 1 \(Frankfurt\): 100.104.160.192/26
    -   Asia Pacific NE 1 \(Tokyo\): 100.104.160.192/26
    The whitelist \(example\) for the RDS console is as follows:

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7547/3010_en-US.png)


