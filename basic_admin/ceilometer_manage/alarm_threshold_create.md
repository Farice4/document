# 创建阈值报警 #

阈值报警，指报警条件会设置某个meter的阈值，根据这个阈值来判断是否达到报警的条件，从而触发进一步的操作。

## 用法 ##

```
ceilometer alarm-threshold-create --name <NAME>
                                    [--project-id <ALARM_PROJECT_ID>]
                                    [--user-id <ALARM_USER_ID>]
                                    [--description <DESCRIPTION>]
                                    [--state <STATE>]
                                    [--severity <SEVERITY>]
                                    [--enabled {True|False}]
                                    [--alarm-action <Webhook URL>]
                                    [--ok-action <Webhook URL>]
                                    [--insufficient-data-action <Webhook URL>]
                                    [--time-constraint <Time Constraint>]
                                    -m <METRIC>
                                    [--period <PERIOD>]
                                    [--evaluation-periods <COUNT>]
                                    [--statistic <STATISTIC>]
                                    [--comparison-operator <OPERATOR>]
                                    --threshold <THRESHOLD>
                                    [-q <QUERY>]
                                    [--repeat-actions {True|False}]
```

* 必选参数：

    * `--name <NAME>`

    即这个警报的name

    * `-m <METRIC>`

    用来评估报警的meter

    * `--threshold <THRESHOLD>`

    用来评估报警的阈值

* 可选参数：

    * `--project-id <ALARM_PROJECT_ID>`

    用来限定这个报警只针对于某个或某些project，可用[xxx,xxx]表示

    * `--user-id <ALARM_USER_ID>`

    用来限定这个报警只针对于某个或某些user，可用[xxx,xxx]表示

    * `--description <DESCRIPTION>`

    警报的描述信息

    * `--state <STATE>`

    警报的当前状态，共三种状态：

        * Insufficient Data：默认状态，数据不足

        * OK：数据充足，但未告警

        * ALARM：告警状态

    该项参数一般在创建的时候不使用。

    * `--enabled {True|False}`

    即是否启用该警报，可在不删除警报的状态下关闭警报

    * `--alarm-action <Webhook URL>`

    当状态为ALARM时的操作，关于这个Webhook URL，可以是一个URL地址，当状态发生时，或进行对这个URL一个POST操作

    * `--ok-action <Webhook URL>`

    当状态为OK时的操作，操作同上

    * `--insufficient-data-action <Webhook URL>`

    当状态为insufficient data时的操作，操作同上

    * `--time-constraint <Time Constraint>`

    时间约束，只有在这个时间约束内，这个报警才会生效；

    可以制定多个时间约束，格式如下：

    `name=<CONSTRAINT_NAME>;start=<CRON>;duration=<SECONDS>;[description=<DESCRIPTION>;[timezone=<IANA Timezone>]]`

    * `--period <PERIOD>`

    用来评估的周期时间

    * `--evaluation-periods <COUNT>`

    用来评估的周期个数

    * `--statistic <STATISTIC>`

    用评估的统计类型，如：'max','min','avg','sum'，'count'，选择一个

    * `--comparison-operator <OPERATOR>`

    用来评估统计的比较方式，如：'lt'（小）、'le'（小于等于）、'eq'（等于）、'ne'（不等于）、'ge'（大于等于）、'gt'（大于），选择一个

    * `-q <QUERY>`

    * `--repeat-actions {True|False}`

    当警报处于某个状态，是否多次执行action

### 示例 ##

* 创建一个阈值报警

当某个vm一次600周内运行时长大于360000时，发出报警。

```
# ceilometer alarm-threshold-create --name instance_time_long -m instance --period 600 --evaluation-period 1 --statistic max --comparison-operator gt --threshold 360000 --alarm-action log://
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
| state                     | insufficient data                                            |
| statistic                 | max                                                          |
| threshold                 | 360000.0                                                     |
| type                      | threshold                                                    |
| user_id                   | 7e2fff4edbad42869051c5e631ca2600                             |
+---------------------------+--------------------------------------------------------------+
```
