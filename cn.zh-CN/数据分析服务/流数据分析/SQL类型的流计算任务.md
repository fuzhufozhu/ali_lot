# SQL类型的流计算任务 {#task_okc_1xh_hfb .task}

下文介绍如何创建一个SQL类型的流计算任务。

1.  在任务管理页面，单击右上角的**创建任务**。 

    任务类型选择**SQL**，输入任务名称和描述，选择在云端或边缘端执行任务。

    单击**确定**，新建一个SQL类型的流计算任务。

     ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22159/154752077321672_zh-CN.png) 

2.  单击**查看**。 

    ![](images/12851_zh-CN_source.png)

3.  在SQL编辑器页面右侧的，**view**页签中单击**添加**，对产品进行建表，方便在SQL中引用，如下图pm\_test.wet（表名.属性）所示。 

    设置view名称并选择产品和设备后单击**确定**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22159/154752077337103_zh-CN.png)

    添加完成view后，在SQL中引用。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22159/154752077337168_zh-CN.png)

4.  在SQL编写页面，输入SQL之后，单击**SQL校验**，会对SQL的正确性进行初步校验。 
    -   如果校验通过，则可以进行下一步。
    -   如果检验失败，则会提示失败。

        请根据提示信息更改SQL内容，若对提示信息有疑问或不知道如何修改，则可以从流数据分析页面，选择一个**组件编排**任务，单击该任务后的查看，进入组件编排，单击**SQL预览**，参考SQL语句。

        根据参考内容重新编写SQL后，再次进行校验，直至校验通过后，进行下一步操作。

        ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/22159/154752077337104_zh-CN.png)

5.  任务设置完毕后，单击**发布**即可进行任务发布。 

    发布之后，后台会开启流计算任务进行实时计算，将计算结果不断的输出到设置的数据源的数据表中。

    **说明：** 若任务在边缘端执行，您还需参考[边缘流计算分析](../../../../../cn.zh-CN/用户指南/流数据分析/分配流数据分析到边缘实例.md#)，完成边缘任务部署。


