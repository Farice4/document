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

    ```
    name=<CONSTRAINT_NAME>;start=<CRON>;duration=<SECONDS>;[description=<DESCRIPTION>;[timezone=<IANA Timezone>]]
    ```

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



### 示例 ##

* 创建一个阈值报警

当某个vm运行时的cpu_util最大值超过５０％,时长180s,发出报警,警告通过日志的形式通知,警告通知的文件为`alarm-notifier.log`

```
# ceilometer alarm-threshold-create --name cpu_high --description "Record about cpu_util value , when the value too high,the alarm is send log message" --alarm-action 'log://' --threshold 50 --period 180 --statistic max --comparison-operator gt -q resource_id=56beeaf4-e833-4787-8ce8-482ae14d87b5 -m cpu_util　--repeat-actions False
+---------------------------+-------------------------------------------------------------------------+
| Property                  | Value                                                                   |
+---------------------------+-------------------------------------------------------------------------+
| alarm_actions             | [u'log:///tmp/alarm.log']                                               |
| alarm_id                  | 11ccc804-0d8b-4a28-b98c-c87410eeaf86                                    |
| comparison_operator       | gt                                                                      |
| description               | Record about cpu_util value , when the value too high,the alarm is send |
|                           | log message                                                             |
| enabled                   | True                                                                    |
| evaluation_periods        | 1                                                                       |
| exclude_outliers          | False                                                                   |
| insufficient_data_actions | []                                                                      |
| meter_name                | cpu_util                                                                |
| name                      | cpu_high                                                                |
| ok_actions                | []                                                                      |
| period                    | 180                                                                     |
| project_id                |                                                                         |
| query                     | resource_id == 56beeaf4-e833-4787-8ce8-482ae14d87b5                     |
| repeat_actions            | False                                                                   |
| state                     | insufficient data                                                       |
| statistic                 | max                                                                     |
| threshold                 | 50.0                                                                    |
| type                      | threshold                                                               |
| user_id                   | 3dbf0919d60d4025842e6ea149e4aeba                                        |
+---------------------------+-------------------------------------------------------------------------+

```

alarm-action action 种类：

* log：指定方式为`log://`，会在Ceilometer的日志文件中打印告警
* http: 指定方式为`http://www.domainname.com/xyz`，会POST请求到该URL
* https: 指定方式为`https://www.domainname.com/xyz`，会POST请求到该URL
* test: 指定方式为`test://`，不要使用这种方式，仅作测试用途，对告警不会有任何效果
* trust+http：指定方式为`trust+http://`，你需要进行额外设置才可以使用
* trust+https：指定方式为`trust+https://`，你需要进行额外设置才可以使用

备注：当警告触发后,警告未排除，会一直触发alarm-action动作
