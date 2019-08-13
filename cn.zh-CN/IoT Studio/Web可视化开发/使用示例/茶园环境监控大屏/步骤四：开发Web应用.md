# 步骤四：开发Web应用 {#task_1198282 .task}

开发一个Web应用，用于展示和查询指定时间段中，设备上报的每小时内的最高温度。

1.  在项目页，选择**Web可视化开发** \> **新建Web应用**。
2.  在Web可视化开发页，鼠标光标移至**选择模板**下的区域，单击出现的**使用该模板开发**按钮。
3.  在**新建Web可视化应用**对话框中，填入应用名称和描述，单击**完成**，新建一个Web可视化应用。
4.  配置应用页面，设置页面背景和分辨率。
5.  配置一个矩形组件，作为其他组件的背景。 

    ![矩形组件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568708851638_zh-CN.png)

6.  配置一个文字组件，用于展示标题。 

    ![文字标题](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568708851639_zh-CN.png)

7.  配置选择设备的下拉框。 
    1.  配置一个文字组件，作为下拉框的标题。
    2.  配置下拉框组件样式。下拉框中，显示设备名称。 

        |参数|说明|
        |:-|:-|
        |列表内容|选择为**设备**，表示下拉框中展示设备名称。|
        |选择产品|选择设备所属的产品。下拉框中，展示该产品下的设备名称。|

        ![下拉框配置](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568708951640_zh-CN.png)

    3.  选择配置栏中的**交互**。
    4.  选择事件为**值改变**；动作为**赋值给变量**，单击**管理变量**。
    5.  单击**新增变量**，新增一个名称为DeviceName的变量。 

        ![新增变量](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568708951644_zh-CN.png)

    6.  单击**配置** \> **赋值**，选择**value**，赋值给变量DeviceName。
    7.  单击**确定**，完成交互动作配置。 

        ![赋值给变量](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568708951645_zh-CN.png)

8.  配置用户设置查询起始时间的时间组件。 
    1.  配置一个文字组件，作为时间组件的标题。
    2.  配置时间组件样式。时间单位选择为**秒**。 

        ![时间组件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568709051641_zh-CN.png)

    3.  配置交互动作，创建一个变量startTime，并配置通过值改变事件，触发交互动作，赋值给变量。
9.  以相同的方法，配置查询结束时间的时间组件，并配置交互动作，赋值给变量endTime。 

    ![事件组件](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568709051643_zh-CN.png)

10. 配置一个折线图组件，用于展示温度数据。 
    1.  配置一个文字组件，作为折线图组件的标题。
    2.  配置折线图组件样式。
    3.  配置折线图组件的数据源为步骤三中创建的HTTP接口，请求参数值设置为前面创建的变量。 

        ![折线图数据](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/961117/156568709151646_zh-CN.png)

11. 单击顶部栏中的**预览**按钮，预览应用。
12. 单击**发布**按钮，发布应用。

