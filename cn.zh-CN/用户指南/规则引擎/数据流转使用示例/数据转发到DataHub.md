# 数据转发到DataHub {#task_p4z_jyz_vdb .task}

您可以使用规则引擎将数据转到DataHub上，再由DataHub将数据流转至实时计算、MaxCompute等服务中，以实现更多计算场景。

在设置转发之前，您需要参见[设置数据流转规则](cn.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)编写SQL，用于处理数据。

1.  单击**数据转发**一栏的**添加操作**，出现添加操作页面。选择**发送数据到DataHub中**。 

    ![数据流转到DataHub](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7546/15663590382843_zh-CN.png)

2.  按照界面提示，设置参数。 
    -   选择操作：选择发送数据到DataHub中。
    -   选择DataHub对应的地域、Project和Topic。选完Topic后，规则引擎会自动获取Topic中的Schema，规则引擎筛选出来的数据将会映射到对应的Schema中。

        **说明：** 

        -   将数据映射到Schema时，需使用`${}`，否则存入表中的将会是一个常量。
        -   Schema与规则引擎的的数据类型必须保持一致，不然无法存储。
    -   角色：授权物联网平台将数据写入DataHub。此处需要您先创建一个具有DataHub写入权限的角色，然后将该角色赋予给规则引擎。这样，规则引擎才可以将处理完成的数据写入DataHub中。

[物联网平台对接大数据平台](../../../../cn.zh-CN/最佳实践/物联网平台对接大数据平台.md#)

