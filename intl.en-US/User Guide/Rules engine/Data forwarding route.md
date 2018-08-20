# Data forwarding route {#concept_xlr_xgp_r2b .concept}

The rule engine only can process device data that is sent to topics. The data forwarding route is different for Basic Edition and Pro Edition devices.

## Data forwarding route of Basic Edition devices {#section_nft_hhp_r2b .section}

The device data of Basic Edition devices passes unaltered to IoT Hub. An example of a data forwarding route is shown in the following figure. 

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17298/15347371838912_en-US.png)

## Data forwarding route of Pro Edition devices {#section_gzm_vhp_r2b .section}

When you create a Pro Edition product, you are required to select the data type as either Do not parse/Custom \(indicating binary data\) or Alink JSON.

-   Do not parse/Custom: The rule engine does not parse binary data and the data passes to the targets. The data forwarding route is the same as that of Basic Edition devices.
-   Alink JSON: Data is first parsed to be Thing Specification Language \(TSL\), and then the rule engine executes the SQL statements for the parsed data.

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17298/15347371838913_en-US.png)

