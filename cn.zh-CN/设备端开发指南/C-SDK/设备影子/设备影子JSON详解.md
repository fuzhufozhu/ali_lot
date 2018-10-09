# 设备影子JSON详解 {#concept_w3l_41v_wdb .concept}

本文档介绍设备影子的JSON格式表达方法。

设备影子JSON文档示例：

```

{
"state" : {
"desired" : {
"color" : "RED",
"sequence" : [ "RED", "GREEN", "BLUE" ]
},
"reported" : {
"color" : "GREEN"
}
},
"metadata" : {
"desired" : {
"color" : {
"timestamp" : 1469564492
},
"sequence" : {
"timestamp" : 1469564492
}
},
"reported" : {
"color" : {
"timestamp" : 1469564492
}
}
},
"timestamp" : 1469564492,
"version" : 1
}
```

JSON属性描述，如下表[表 1](#table_mwk_fcx_wdb)所示。

|属性|描述|
|:-|:-|
|desired|设备的预期状态。仅当设备影子文档具有预期状态时，才包含desired部分。应用程序向desired部分写入数据，更新事物的状态，而无需直接连接到该设备。

|
|reported|设备的报告状态。设备可以在reported部分写入数据，报告其最新状态。应用程序可以通过读取该参数值，获取设备的状态。

JSON文档中也可以不包含reported部分，没有reported部分的文档同样为有效影子JSON文档。

|
|metadata|当用户更新设备状态文档后，设备影子服务会自动更新metadata的值。设备状态的元数据的信息包含以 Epoch 时间表示的每个属性的时间戳，用来获取准确的更新时间。

|
|timestamp|影子文档的最新更新时间。|
|version|用户主动更新版本号时，设备影子会检查请求中的version值是否大于当前版本号。如果大于当前版本号，则更新设备影子，并将version值更新到请求的版本中，反之则会拒绝更新设备影子。

该参数更新后，版本号会递增，用于确保正在更新的文档为最新版本。

|

**说明：** 

设备影子支持数组。更新数组时必须全量更新，不能只更新数组的某一部分。

更新数组数据示例：

-   初始状态：

    ```
    
    {
    "reported" : { "colors" : ["RED", "GREEN", "BLUE" ] }
    }
    ```

-   更新：

    ```
    
    {
    "reported" : { "colors" : ["RED"] }
    }
    ```

-   最终状态：

    ```
    
    {
    "reported" : { "colors" : ["RED"] }
    }
    ```


