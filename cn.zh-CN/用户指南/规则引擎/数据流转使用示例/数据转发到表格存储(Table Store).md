# 数据转发到表格存储\(Table Store\) {#task_ay5_3x5_vdb .task}

您可以使用规则引擎数据流转功能，将数据转发到表格存储（Table Store）中存储。

在设置转发之前，

-   在物联网平台中，创建规则和编写处理消息数据的SQL。

    请参见[设置数据流转规则](cn.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)。

    本文示例的规则，定义了如下SQL语句：

    ```
    SELECT deviceName as deviceName, items.PM25.value as PM25, items. WorkMode.value as WorkMode 
    FROM "/sys/a1ktuxe****/aircleanerthing/event/property/post" WHERE
    ```

-   在表格存储中，创建接收数据的实例和数据表。

    表格存储使用指南，请参见[表格存储文档](https://help.aliyun.com/product/27278.html)。


1.  在规则的数据流转详情页，单击**数据转发**一栏的**添加操作**。选择**存储到表格存储\(Table Store\)**。 

    **说明：** 不支持二进制格式的数据转发至表格存储。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/15562703812643_zh-CN.png)

2.  按照界面提示设置参数，然后单击**确定**。 

    |参数|说明|
    |:-|:-|
    |选择操作|选择为**存储到表格存储\(Table Store\)**。|
    |地域|选择接收数据的表格存储实例的地域。|
    |实例|选择接收数据的表格存储实例。|
    |数据表|选择接收数据的数据表。|
    |主键|配置表格存储数据表主键对应的值，需设置为规则SQL中SELECT的某字段值。数据流转时，该值将被存为主键对应的值。**说明：** 

    -   支持配置为变量格式`${}`，如$\{deviceName\}，表示该主键对应的值为消息中`deviceName`的值。
    -   如果主键类型是自增列，这一列主键无需填值，表格存储会自动生成这一主键列的值。所以，自增列主键值，系统已自动设置为`AUTO_INCREMENT`，且不能编辑。

更多自增列主键说明，请参见[主键列自增](https://help.aliyun.com/document_detail/47745.html)。

|
    |角色|授权物联网平台将数据写入表格存储。需要您进入[访问控制RAM控制台](https://ram.console.aliyun.com/roles)创建一个具有表格存储写入权限的角色，然后将该角色赋予给物联网平台。

|

3.  数据转发操作配置完成后，回到数据流转列表页，单击规则对应的**启动**操作按钮。 规则启动后，SQL语句中定义的Topic有消息发布时，SELECT字段定义的消息数据将被流转至表格存储数据表中。
4.  模拟推送数据，测试数据流转。 
    1.  在物联网平台控制台左侧导航栏，选择**监控运维** \> **在线调试**。 
    2.  选择调试用的设备，使用**虚拟真实设备**推送模拟数据。请参见[虚拟设备调试](cn.zh-CN/用户指南/监控运维/在线调试/虚拟设备调试.md#)。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/155627038241753_zh-CN.png)

    3.  数据推送成功后，在表格存储接收数据的数据表的数据管理页，查看是否成功接收到指定数据。 

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/155627038241760_zh-CN.png)


