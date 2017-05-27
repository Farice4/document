# 接收器

接收器是和集群相关联的一个对外提供执行相应操作的url，通过请求这个url来执行相应的操作，与监控和告警结合实现集群的自动伸缩。

## 创建接收器

使用命令`senlin receiver-create`创建一个接收器。

- -t 接收器类型
- -c 接收器相关联的集群
- -a 触发接收器后执行的动作
- -P 附加参数

```
$ senlin receiver-create -t webhook -c cluster_1 -a CLUSTER_SCALE_OUT -P count=1 scale-out
| Property   | Value                                                                                                          |
+------------+----------------------------------------------------------------------------------------------------------------+
| action     | CLUSTER_SCALE_OUT                                                                                              |
| actor      | {                                                                                                              |
|            |   "trust_id": "1eb1382ff73e43c28c6051fc011a935d"                                                               |
|            | }                                                                                                              |
| channel    | {                                                                                                              |
|            |   "alarm_url": "http://10.10.10.147:8778/v1/webhooks/1a731705-fc24-4ab4-93c1-6bce4f204887/trigger?V=1&count=1" |
|            | }                                                                                                              |
| cluster_id | 4741cacc-7fcd-4622-9788-db74075c96ee                                                                           |
| created_at | 2017-05-27T05:58:58Z                                                                                           |
| domain_id  | -                                                                                                              |
| id         | 1a731705-fc24-4ab4-93c1-6bce4f204887                                                                           |
| location   | -                                                                                                              |
| name       | scale-out                                                                                                      |
| params     | {                                                                                                              |
|            |   "count": "1"                                                                                                 |
|            | }                                                                                                              |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d                                                                               |
| type       | webhook                                                                                                        |
| updated_at | -                                                                                                              |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e                                                                               |
+------------+----------------------------------------------------------------------------------------------------------------+
```

上面的命令执行后会返回一个alarm_url地址，请求该地址触发接收器后，集群cluster_1会执行集群扩展操作，向集群中增加1个节点。

一般情况下接收器和ceilometer一起使用，使用ceilometer创建一个监控项，当有告警时自动触发接收器执行集群的伸缩操作。

下面命令创建了一个告警项，当集群在1分钟内cpu平均使用率大于70%时触发接收器执行集群扩展操作。

```
$ ceilometer alarm-create --name cpu-high --meter-name cpu_util --threshold 70 --statistic avg --comparison-operator gt --period 60 --evaluation-periods 1 --alarm-action http://10.10.10.147:8778/v1/webhooks/1a731705-fc24-4ab4-93c1-6bce4f204887/trigger?V=1&count=1 --repeat-actions False
```

在云主机内，使用`cat /dev/zero > /dev/null`命令增加cpu使用率，等待一段时间，当ceilometer监控到云主机cpu使用率过高时会触发告警，执行scale-out操作。

## 列出已存在的接收器

使用`senlin receiver-list`命令列出已经创建了的接收器的列表。

- -f 根据给定条件筛选
- -l 最多显示几条结果
- -o 根据给定项排序
- -F 显示完整id

```
senlin receiver-list
+----------+-----------+---------+------------+-------------------+----------------------+
| id       | name      | type    | cluster_id | action            | created_at           |
+----------+-----------+---------+------------+-------------------+----------------------+
| 1a731705 | scale-out | webhook | 4741cacc   | CLUSTER_SCALE_OUT | 2017-05-27T05:58:58Z |
+----------+-----------+---------+------------+-------------------+----------------------+
```

## 查看接收器

使用`senlin receiver-show`命令查看已经创建了的接收器的详细信息。

```
$ senlin receiver-show scale-out
+------------+----------------------------------------------------------------------------------------------------------------+
| Property   | Value                                                                                                          |
+------------+----------------------------------------------------------------------------------------------------------------+
| action     | CLUSTER_SCALE_OUT                                                                                              |
| actor      | {                                                                                                              |
|            |   "trust_id": "1eb1382ff73e43c28c6051fc011a935d"                                                               |
|            | }                                                                                                              |
| channel    | {                                                                                                              |
|            |   "alarm_url": "http://10.10.10.147:8778/v1/webhooks/1a731705-fc24-4ab4-93c1-6bce4f204887/trigger?V=1&count=1" |
|            | }                                                                                                              |
| cluster_id | 4741cacc-7fcd-4622-9788-db74075c96ee                                                                           |
| created_at | 2017-05-27T05:58:58Z                                                                                           |
| domain_id  | -                                                                                                              |
| id         | 1a731705-fc24-4ab4-93c1-6bce4f204887                                                                           |
| location   | -                                                                                                              |
| name       | scale-out                                                                                                      |
| params     | {                                                                                                              |
|            |   "count": "1"                                                                                                 |
|            | }                                                                                                              |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d                                                                               |
| type       | webhook                                                                                                        |
| updated_at | -                                                                                                              |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e                                                                               |
+------------+----------------------------------------------------------------------------------------------------------------+
```

## 删除接收器

使用`senlin receiver-delete`命令删除指定接收器。

```
$ senlin receiver-delete scale-out
Receivers deleted: ['scale-out']
```
