# Node.js脚本 {#concept_644713 .concept}

如果IoT Studio平台提供的节点不能满足您的需求，您可以使用Node.js脚本节点，编写JavaScript代码来灵活定制功能逻辑。目前支持Node.js 6.10版本。

## 编码帮助 {#section_wpr_i5j_ccs .section}

Node.js配置页：

![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/518522/156144686049448_zh-CN.png)

-   使用动态参数。

    在脚本中，可使用平台内置变量。平台已内置以下全局变量：

    -   来自上个节点的输出数据：payload。支持使用`payload.payload对象中的某个key`来访问指定key的数据。
    -   来自输入节点的数据：query。比如，HTTP请求节点的入参，设备触发节点的设备数据，支持使用`query.参数名`来访问指定数据。
    -   来自指定节点的输出数据：`node.节点ID`。支持使用`node.节点ID.节点输出对象中的某个key`来访问指定key的数据。
    示例：

    ``` {#codeblock_aei_vw3_cde}
    {
       "productKey": "{{payload:productKey}}",  // 上一个节点的输出为：{productKey: '值'}，取productKey的值
       "deviceName": "{{query.deviceName}}", // API请求节点的入参中，定义了一个名称为deviceName的入参，取入参deviceName的值
       "pageNum": "{{node.node_399591c0.pageNum}}" // 节点node_399591c0的输出为pageNum，取pageNum的值
    }
    ```

    如果需要调用某参数的子集，可按如下示例方式调用：

    使用`{{payload.props.PM10.value}}`，表示上一个节点props对象中属性PM10的值。

    使用`{{query.deviceContext.deviceName}}`，表示第一个节点的输出内容中deviceContext对象的deviceName变量。

-   日志输出。

    可以使用`console.log`输出日志。可以在调试信息中查看日志数据。使用示例如下：

    ``` {#codeblock_4wq_05e_2gy}
    let name = 'Jack';
    console.log('Hello', name);
    ```


## 约束与限制 {#section_ziw_vf8_fmg .section}

|项目|说明|
|:-|:-|
|Date|服务编排最终会运行在阿里云函数计算\(Function Compute\)上。函数计算使用的是UTC时间，因此使用Date对象时，请注意当前时区和UTC时间的差异。|
|NPM库| 脚本节点中已经内置了一些NPM库，可以直接require调用。系统内置库：aliyun-api-gateway、axios、lodash、moment、和uuid。

 您也可以安装第三方库：在扩展库管理中，搜索支持的NPM库模块，然后单击**安装**。使用require方式引入模块。NPM库具体使用指南，请参见本文章节：使用外部扩展库。

 |
|变量|变量必须符合ECMAScript2015严格模式下变量的命名规范。 请勿在脚本中定义使用包含循环引用的变量。

 不能使用以下关键词命名变量：

 abstract、boolean、break、 byte、case、catch、char、class、continue、const、debugger、default、delete、do、double、else、enum、export、extends、false、finally、for、function、goto、if、import、implements、in、instance、of、int、interface、let、 long、native、new、null、package、private、protected、public、return、short、static、super、switch、synchronized、this、throw、throws、transient、try、type of、var、void、volatile、while、with、yield

 |

## 使用外部扩展库 {#section_vxv_mix_173 .section}

使用Node.js脚本节点编写代码过程中，除了可以使用节点中已内置了NPM库，您还可以安装支持的外部NPM库。

1.  在节点配置下，单击**扩展库管理**。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/518522/156144686050064_zh-CN.png)

2.  搜索您需要的外部库，单击其对应的**安装**按钮。

    扩展库安装完成后，将展示在已安装库列表中。

3.  在编写代码时，通过require方式使用扩展库。

    ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/518522/156144686050066_zh-CN.png)


## 附录：代码示例 {#section_cgo_ctv_paq .section}

``` {#codeblock_dtc_m6n_6he}
/**
 * @param {Object} payload 上一节点的输出
 * @param {Object} node 指定某个节点的输出
 * @param {Object} query 服务流第一个节点的输出
 */
module.exports = function(payload, node, query) {
  database = [
    ["A", 11, 111],
    ["B", 22, 222],
    ["C", 33, 333],
    ["D", 44, 444],
    ["E", 55, 555],
    ["F", 11, 111],
    ["G", 22, 222],
    ["H", 33, 333],
    ["I", 44, 444],
    ["J", 55, 555],
    ["K", 11, 111],
    ["L", 22, 222],
    ["M", 33, 333],
    ["M", 44, 444],
    ["O", 55, 555],
  ];
  let arr = [];
  for(let i=0;i<query.column;i++)
    {arr[i]= database[i];
    }
/**
 * 此时传递的参数payload被赋值为arr，传递的二维数组含有N个数据，其中N通过API入参传递过来
 */
  return arr;
}
```

