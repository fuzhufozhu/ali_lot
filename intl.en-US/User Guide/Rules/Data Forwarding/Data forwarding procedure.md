# Data forwarding procedure {#concept_xlr_xgp_r2b .concept}

Data forwarding provided by the rules engine function can only process data that is published to topics. This topic describes the procedure of data forwarding and the formats of the data at different stages during data forwarding.

## Custom topics {#section_nft_hhp_r2b .section}

Data published to custom topics is forwarded transparently to the IoT Platform by data forwarding. The structure of the data is not changed. The following figure shows the data forwarding procedure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17298/15507217928912_en-US.png)

## System topics {#section_gzm_vhp_r2b .section}

Data published to system topics is in the Alink JSON format. During data forwarding, the data is parsed according to the TSL and then processed by the SQL statements of a rules engine. For more information about the data format, see [Data format \(Pro Edition\)](reseller.en-US/User Guide/Rules/Data Forwarding/Data format (Pro Edition).md#). The following figure shows the data forwarding procedure:

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/17298/15507217928913_en-US.png)

**Note:** During data forwarding, parameter params in the payload is replaced by parameter items after the data is parsed according to the TSL.

