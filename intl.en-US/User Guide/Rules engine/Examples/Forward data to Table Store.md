# Forward data to Table Store {#task_ay5_3x5_vdb .task}

You can configure the rules engine to forward the processed data to Table Store.

Before configuring forwarding, follow the instructions in [Create and configure a rule](intl.en-US/User Guide/Rules engine/Create and configure a rule.md#) to write a SQL script to filter the data.

1.   Click **Add Operation** next to **Data Forwarding** to open the Add Operation page. Select **Save to Table Store** . ![](http://static-aliyun-doc.oss-cn-hangzhou.aliyuncs.com/assets/img/7545/15349063542643_en-US.png) 
2.   Follow the instructions on the page to configure the parameters. 
    -   Select Operation: Select Table Store.
    -   Region, Instance, and Table: Specify each of these fields for the table to which you want to forward data.
    -   Primary Key Field: All tables in Table Store have primary key columns. After you have selected the table to forward data, the console automatically reads the primary key fields of this table. You need to configure the values of the primary key fields.
    -   Role: Grant IoT Platform permission to write data to Table Store. First create a role that includes permission to write data to Table Store and assign this role to the rules engine. The rules engine can now write processed data to a table.

**Example**

The JSON data record `{"device":"bike","product":"xxx","data2":[{...}]}` is extracted using SQL. This JSON data record needs to be stored in Table Store. The primary key columns in the destination table are `device`, `product`, and `id` .

Configuration and effects:

1.  Set the value of the primary key field “device” to `${device}` in the console. When a message arrives that triggers the forwarding rule, the value of the device field in the JSON data record will be saved under the device column in the destination table. The preceding configuration and effects resemble those for the primary key field "product".

    **Note:** `${}` is an escape character. If you do not use this escape character, the constant you specify as the value of the primary key field will be saved to the primary key column.

2.  The forwarding rule will automatically detect the auto-increment column. The auto-increment column will be automatically assigned a unique value every time a new record is inserted into the table. The values in this column cannot be edited.
3.  IoT Platform can automatically parse values of the non-primary key fields included in the JSON data record and create corresponding columns for the destination table . In this example, two columns, data1 and data2, will automatically be created, and the corresponding values will be saved under each column.

    **Note:** Currently, only top-level JSON structure can be parsed. Parsing of nested JSON structures is not supported. Therefore, in this example, the entire JSON object with its nested structure will be saved under the data2 column. The nested JSON structure will not be further parsed. No additional columns will be created to save the nested elements.


