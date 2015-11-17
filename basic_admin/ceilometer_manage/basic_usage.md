# 命令行基本用法 #

ceilometer很多操作都是通过命令行实现。在多数的查询操作中，会有可选的命令行参数；

`-q <QUERY>, --query <QUERY>`

即查询条件，用于过滤出满足条件的结果。
其中的`<QUERY>`可以是单个或多个表达式，形式如：
* `attr1=value1`（单个表达式）
* `attr1=value1;attr2=value2`（多个表达式）

其中的attr为要查询对象的属性，例如resource对象的属性有project_id，resource_id，source，user_id。

> ###### 注意
> * 当只有单个条件的时候，参数形如：`-q attr1=value1`
* 当有多个条件的时候，参数形如：`-q "attr1=value1;attr2=value2"`，必须添加引号（"）。

> #### 重要
> 由于ceilometer中的数据量很大，所以在查询中，应该尽量的多写查询条件，可以减少很多没必要的迭代操作，大幅度的缩小查询时间。
