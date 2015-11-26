# 查看所有报警 #

Alarm 包括 threshold alarm （阈值告警）和 combination （组合告警）两种类型。

* Threshold alarm 根据监控指标的阈值去判断alarm的状态，它只是针对某一个监控指标建立alarm。它包括几个要素：

    * 一个静态阈值和比较方法

    * 指定的 meter statistic

    * 比较的时间窗

* Combination alarm 根据多个alarm的状态来判断自己的状态的，多个alarm之间是or/and的关系，这相当于是对多个监控指标建立了一个alarm

## 用法 ##

`ceilometer alarm-list [-q <QUERY>]`

可选参数：

`-q <QUERY>`

查询条件，具体可以参考 [基本使用](./basic_usage.md)

可选的条件属性有：

| attr | value eg |
| --- | --- |
| alarm_id | 9db1b034-5605-4643-a564-4dcf174ce0b7 |
| name | cpu_util_high |
| state | insufficient data |
| enabled | true |

告警状态:

|状态|备注|
|-----|----|
|ok   |正常状态，数据充足未告警|
|alarm|告警状态|
|insufficient data|数据不充足|

## 示例 ##

查看所有报警

```
# ceilometer alarm-list
+--------------------------------------+--------------------+-------+---------+------------+-------------------------------------+------------------+
| Alarm ID                             | Name               | State | Enabled | Continuous | Alarm condition                     | Time constraints |
+--------------------------------------+--------------------+-------+---------+------------+-------------------------------------+------------------+
| 9db1b034-5605-4643-a564-4dcf174ce0b7 | cpu_util_high      | ok    | True    | True       | cpu_util > 30.0 during 1 x 60s      | None             |
| c8c881cd-453c-45d7-9970-93b787f47e40 | instance_time_long | ok    | True    | False      | instance > 360000.0 during 1 x 600s | None             |
+--------------------------------------+--------------------+-------+---------+------------+-------------------------------------+------------------+
```

