# 创建组合报警 #

根据多个alarm的状态来判断自己的状态的，多个alarm之间是or/and的关系，这相当于是对多个监控指标建立了一个alarm。

## 用法 ##

```
ceilometer alarm-combination-create --name <NAME>
                                    [--project-id <ALARM_PROJECT_ID>]
                                    [--user-id <ALARM_USER_ID>]
                                    [--description <DESCRIPTION>]
                                    [--state <STATE>]
                                    [--enabled {True|False}]
                                    [--alarm-action <Webhook URL>]
                                    [--ok-action <Webhook URL>]
                                    [--insufficient-data-action <Webhook URL>]
                                    [--time-constraint <Time Constraint>]
                                    --alarm_ids <ALARM IDS>
                                    [--operator <OPERATOR>]
                                    [--repeat-actions {True|False}]
```

针对参数这里需要说明有如下，

* 必选参数：

    * `--alarm_ids <ALARM IDS>`

    阈值警报的id，至少2个，形式如下：

    `--alarm_ids xxx --alarm_ids xxx`

* 可选参数：

    * `--operator <OPERATOR>`

    指多个阈值警报之间的关系，如and/or，从而触发报警

关于其他的参数，请查阅 [这里](./alarm_threshold_create.md)

## 示例 ##

* 创建一个组合报警

创建一个报警，当cpu_util_high和instance_time_long同时满足的时候，触发警报。

```
# ceilometer alarm-combination-create --alarm_ids 9db1b034-5605-4643-a564-4dcf174ce0b7 --alarm_ids c8c881cd-453c-45d7-9970-93b787f47e40 --operator and --alarm-action log:// --name cpu_and_time --repeat-actions True
+---------------------------+-------------------------------------------------------------------+
| Property                  | Value                                                             |
+---------------------------+-------------------------------------------------------------------+
| alarm_actions             | []                                                                |
| alarm_id                  | f4d2fe6f-e363-4f29-8898-a456717cc3cb                              |
| alarm_ids                 | [u'9db1b034-5605-4643-a564-4dcf174ce0b7', u'c8c881cd-453c-        |
|                           | 45d7-9970-93b787f47e40']                                          |
| description               | Combined state of alarms 9db1b034-5605-4643-a564-4dcf174ce0b7 and |
|                           | c8c881cd-453c-45d7-9970-93b787f47e40                              |
| enabled                   | True                                                              |
| insufficient_data_actions | []                                                                |
| name                      | cpu_and_time                                                      |
| alarm_actions                | [u'log://']                                                       |
| operator                  | and                                                               |
| project_id                |                                                                   |
| repeat_actions            | False                                                             |
| state                     | insufficient data                                                 |
| type                      | combination                                                       |
| user_id                   | 7e2fff4edbad42869051c5e631ca2600                                  |
+---------------------------+-------------------------------------------------------------------+
```
