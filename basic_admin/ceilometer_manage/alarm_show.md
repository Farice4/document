# 查看报警详情 #

详细列出某个报警的属性。

## 用法 ##

`ceilometer alarm-show [<ALARM_ID>]`

必选参数：

* `<ALARM_ID>`

警报的id

## 示例 ##

查看某个警报的详情

```
# ceilometer alarm-show c8c881cd-453c-45d7-9970-93b787f47e40
+---------------------------+--------------------------------------------------------------+
| Property                  | Value                                                        |
+---------------------------+--------------------------------------------------------------+
| alarm_actions             | [u'log://']                                                  |
| alarm_id                  | c8c881cd-453c-45d7-9970-93b787f47e40                         |
| comparison_operator       | gt                                                           |
| description               | Alarm when instance is gt a max of 360000.0 over 600 seconds |
| enabled                   | True                                                         |
| evaluation_periods        | 1                                                            |
| exclude_outliers          | False                                                        |
| insufficient_data_actions | []                                                           |
| meter_name                | instance                                                     |
| name                      | instance_time_long                                           |
| ok_actions                | []                                                           |
| period                    | 600                                                          |
| project_id                |                                                              |
| query                     |                                                              |
| repeat_actions            | False                                                        |
| state                     | ok                                                           |
| statistic                 | max                                                          |
| threshold                 | 360000.0                                                     |
| type                      | threshold                                                    |
| user_id                   | 7e2fff4edbad42869051c5e631ca2600                             |
+---------------------------+--------------------------------------------------------------+
```

