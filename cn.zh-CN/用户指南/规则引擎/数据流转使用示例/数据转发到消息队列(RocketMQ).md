# 数据转发到消息队列\(RocketMQ\) {#task_yvk_2c5_vdb .task}

您可以使用规则引擎，将物联网平台数据转发到消息队列（RocketMQ）中存储。从而实现消息从设备、物联网平台、RocketMQ到应用服务器之间的全链路高可靠传输能力。

在设置转发之前，您需

-   参见[设置数据流转规则](cn.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)，创建规则和编写用于筛选转发数据的SQL。
-   购买消息队列（RocketMQ）实例，并创建用于接收数据Topic。RocketMQ使用方法，请参见[RocketMQ订阅消息](https://help.aliyun.com/document_detail/34411.html)。

1.  在规则详情页，单击**数据转发**一栏的**添加操作**。
2.  在添加操作对话框中，选择操作为**发送数据到消息队列\(RocketMQ\)中**，并按照界面提示，设置参数。 

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7544/15586920092637_zh-CN.png)

    |参数|说明|
    |:-|:-|
    |选择操作|数据转发目标云产品。选择为发送数据到消息队列\(RocketMQ\)中。|
    |地域|选择RocketMQ Topic所在地域。|
    |实例|选择RocketMQ Topic所属的实例。 若无RocketMQ 实例，单击**创建实例**到消息队列控制台创建实例。请参见[消息队列文档](https://help.aliyun.com/product/29530.html)。

 |
    |Topic|选择写入物联网平台备数据的RocketMQ Topic。 若无RocketMQ Topic，单击**创建Topic**在消息队列控制台，创建用于接收物联网平台数据的Topic。

 |
    |Tag|（可选）设置标签。 设置标签后，所有通过该操作流转到RocketMQ对应Topic里的消息都会携带该标签。您可以在RocketMQ消息消费端，通过标签进行消息过滤。

 标签长度限制为128字节。可以输入常量或变量。变量格式为$\{key\}，代表SQL处理后的JSON数据中key对应的value值。

 |
    |授权|授予物联网平台数据写入RocketMQ的权限。 如您还未创建相关角色，请单击**创建RAM角色**进入RAM控制台，创建角色和授权策略。如需帮助，请参见[管理 RAM 角色](https://help.aliyun.com/document_detail/93691.html)。

 |

3.  回到数据流转列表页签，单击规则对应的**启动**按钮启动规则。 规则启动之后，物联网平台即可将数据写入RocketMQ的Topic。
4.  测试。 

    向规则SQL中定义的Topic发布一条消息，然后到RocketMQ控制台消息查询页，查看是否成功接收到消息。


