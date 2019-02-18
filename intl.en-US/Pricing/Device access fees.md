# Device access fees {#concept_msp_l2g_ffb .concept}

The device access fee is calculated based on the duration that devices are connected to IoT Platform. These fees are not charged if a device is connected to IoT Platform over CoAP or HTTP, or if the device is a sub-device within a topological relationship. The device access fee supports the Pay-As-You-Go billing method and has no minimum charge fee.

## Billing method {#section_bsh_jlh_ffb .section}

Billing details

-   Fees are calculated based on the duration of the connection in minutes.

    The duration starts when a device successfully connects to IoT Platform and ends when the device disconnects from IoT Platform. This time period is the duration of the connection.

-   The duration is calculated by minutes. If the duration of the connection is less than one minute, the duration is calculated as one minute.

    For example, if a device gets connected to IoT Platform at 18:23:35 2019-01-21 and disconnects from IoT Platform at 18:24:10 on the same day, the duration is calculated as two minutes.

-   If a device connects to and disconnects from IoT Platform multiple times within one minute, the duration is calculated as one minute.

    For example, if a device gets connected to IoT Platform at 18:23:15 2019-01-21, disconnects at 18:23:35, reconnects to IoT Platform at 18:23:40, and disconnects at 18:23:59 again on the same day, the duration is one minute. The device connects to and disconnects from IoT Platform multiple times within one minute. The duration is calculated as one minute.

-   The first 1 million minutes for each month are free of charge. The free quota takes effect at 00:00:00 on the first day of each month. Unused quota cannot be carried over to the next month. The fees are calculated when the duration exceeds 1 million minutes for the month.

Pricings

|Duration \(Minutes/Month\)|Unit price \(Dollar/1,000,000 Minutes\)|
|:-------------------------|:--------------------------------------|
|N ≤ 1,000,000|0|
|1,000,000 < N|0.3|

Billing dates

-   The device access fee is calculated and charged on a daily basis.
-   Fees are rounded up to the nearest cent.

**Note:** This document is for reference only. Refer to your bill for accurate information.

