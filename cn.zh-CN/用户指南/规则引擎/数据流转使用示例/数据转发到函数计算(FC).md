# 数据转发到函数计算\(FC\) {#concept_a1m_ttj_wdb .concept}

您可以使用规则引擎数据流转，将数据转发至函数计算（FC）中，然后由函数计算运行函数脚本进行业务处理。

示例流程如下图：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973033_zh-CN.png)

## 操作步骤 {#section_pgq_g5j_wdb .section}

1.  登录函数计算控制台，创建服务与函数。
    1.  创建服务。其中，服务名称必须填写，其余参数请根据您的需求设置。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973039_zh-CN.png)

    2.  创建服务成功后，创建函数。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973036_zh-CN.png)

    3.  选择函数模板，此处以空白函数模板为示例。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973037_zh-CN.png)

    4.  设置函数参数。

        此示例设置函数的逻辑为直接在函数计算中显示获取的数据。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973038_zh-CN.png)

        |参数|说明|
        |:-|:-|
        |所在服务|选择函数所在的服务。|
        |函数名称|设置函数名称。|
        |运行环境|设置函数运行的环境，此示例中选择java8。|
        |代码配置|上传代码。|
        |函数入口|设置您的函数在函数计算系统运行的调用入口。此示例中设置为com.aliyun.fc.FcDemo::handleRequest。|
        |函数执行内存|根据您的业务情况设置函数执行内存。|
        |超时时间|设置超时时间。|

        更详细的函数计算使用帮助，请参见[函数计算](https://www.alibabacloud.com/help/product/50980.htm)设置。

    5.  函数执行验证。

        函数成功创建后，可以直接在函数计算的控制台执行，以验证函数执行情况。函数计算会直接将函数的输出和请求的相关信息打印在控制台上。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973040_zh-CN.png)

2.  在物联网平台控制台，创建规则，编写筛选消息数据的SQL。请参见[设置规则引擎](intl.zh-CN/用户指南/规则引擎/数据流转/设置数据流转规则.md#)。
3.  在规则的数据流转详情页，单击**数据转发**一栏的**添加操作**，并在弹出的添加操作页面中设置参数。

    **说明：** JSON格式和二进制格式都支持转发到函数计算中

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973034_zh-CN.png)

    |参数|说明|
    |:-|:-|
    |选择操作|选择**发送数据到函数计算\(FC\)中**。|
    |地域|选择接收数据的函数计算服务地域。 **说明：** 目前支持转发数据至函数计算的地域包括：中国（上海）、新加坡和日本（东京）。

 |
    |服务|选择接收数据的函数计算服务。|
    |函数|选择接收数据的函数。|
    |授权|授予物联网平台向函数计算写入数据的权限。 如您还未创建相关角色，请单击**创建RAM角色**进入RAM控制台，创建角色和授权策略。如需帮助，请参见[管理 RAM 角色](https://www.alibabacloud.com/help/doc-detail/93691.htm)。

 |

4.  在数据流转列表页签下，单击规则对应的**启动**按钮启动规则。

    规则启动后，物联网平台就会将相关数据发送至函数计算。

5.  测试。

    向规则SQL中定义的Topic发送一条测试消息，然后再到函数计算控制台去查看是否有相关记录。

    函数计算控制台针对函数的执行情况有监控统计。统计有大概5分钟的延时，可以通过云监控大盘查询函数的执行情况。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7550/15586918973035_zh-CN.png)


