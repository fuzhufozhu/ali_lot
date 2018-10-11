# 数据转发到MQ {#task_yvk_2c5_vdb .task}

您可以使用规则引擎，将数据转发到消息队列（以下简称为MQ）中，从而实现消息依次从设备、物联网平台、MQ到应用服务器之间的全链路高可靠传输能力。

在设置转发之前，您需要参考[设置规则引擎](intl.zh-CN/用户指南/规则引擎/设置规则引擎.md#)编写SQL完成对数据的处理。

1.   单击**数据转发**一栏的**添加操作**，出现添加操作页面。选择**发送数据到消息队列\(Message Queue\)中**。 ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7544/15392477362637_zh-CN.png) 
2.   按照界面提示，设置参数。 

    -   选择操作：选择MQ。
    -   地域：选择MQ Topic所在地域。
    -   Topic：根据业务选择需要写入数据的Topic。
    -   授权：授权平台将数据写入MQ Topic的权限。

        **说明：** MQ非铂金实例的Topic不支持跨地域的访问写入，此时需保证规则引擎与MQ Topic在同一地域下。若您已购买并使用铂金实例，则无需关心地域问题。

    转入MQ的规则启动之后，平台即可将数据写入MQ的Topic，您可以使用MQ实现消息收发，具体使用方法，请参考[MQ订阅消息](https://www.alibabacloud.com/help/doc-detail/34411.htm)。


