# 云主机高可用自动迁移测试

云主机高可用自动迁移测试, 模拟手动关闭云主机所在计算节点, 测试云主机自迁移


## 环境准备

* 检测consul 服务状态

检测consul 集群的状态, leader选举状态(检测命令见前面章节的云主机高可用consul集群介绍)

* 检测autoevacuate服务状态

检测autoevacuate服务状态, 检测autoevacuate 所在consul leader节点的日志, 日志中会记录检测是否正常, 计算节点检测信息

```
# systemctl status eayunstack-auto-evacuate (查看服务状态)
# /var/log/autoevacuate/auto-novaevacuate.log (查看日志)

```

* 确保环境中的nova 服务正常

```
# nova service-list
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+
| Id | Binary           | Host              | Zone     | Status  | State | Updated_at                 | Disabled Reason |
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+
| 1  | nova-consoleauth | node-15.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 2  | nova-scheduler   | node-15.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 3  | nova-conductor   | node-15.eayun.com | internal | enabled | up    | 2016-12-12T08:53:51.000000 | -               |
| 4  | nova-cert        | node-15.eayun.com | internal | enabled | up    | 2016-12-12T08:53:54.000000 | -               |
| 7  | nova-consoleauth | node-17.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 10 | nova-scheduler   | node-17.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 13 | nova-conductor   | node-17.eayun.com | internal | enabled | up    | 2016-12-12T08:53:53.000000 | -               |
| 19 | nova-consoleauth | node-18.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 22 | nova-scheduler   | node-18.eayun.com | internal | enabled | up    | 2016-12-12T08:53:52.000000 | -               |
| 25 | nova-conductor   | node-18.eayun.com | internal | enabled | up    | 2016-12-12T08:53:54.000000 | -               |
| 31 | nova-cert        | node-17.eayun.com | internal | enabled | up    | 2016-12-12T08:53:51.000000 | -               |
| 34 | nova-cert        | node-18.eayun.com | internal | enabled | up    | 2016-12-12T08:53:54.000000 | -               |
| 37 | nova-compute     | node-14.eayun.com | nova     | enabled | up    | 2016-12-12T08:53:49.000000 | -               |
| 41 | nova-compute     | node-24.eayun.com | nova     | enabled | up    | 2016-12-12T08:53:54.000000 | -               |
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+

```
确保controller节点服务正常, compute节点上的nova-compute Status 为enabled, State 为up状态


## 计算节点宕机测试

* 查询准备手动宕机的计算节点虚拟机信息

```
# nova list --host node-24.eayun.com   (查询准备手动宕机的node-24节点上的虚拟机)

# nova show 云主机uuid   (查询某一个云主机所在compute节点)
```

* 手动关闭node-24 节点

autoevacuate下auto-novaevacuate.log日志记录

```
2016-08-18 17:40:15,267 - compute - INFO -192.168.1.243 power status is: OFF
2016-08-18 17:40:28,624 - compute - INFO -node-24.eayun.com fence successed
```

其中的192.168.1.243为node-24节点的ipmi 电源管理检测地址, 同时能够看到autoevacuate对node-24 进行了fence隔离

* 云主机自动迁移

autoevacuate 通过调用nova host-evacuate实现node-24节点云主机撤离

autoevacuate下auto-novaevacuate.log日志记录

```
2016-08-18 17:40:29,639 - compute - WARNING -8e27d16f-cea8-4324-8fef-6511688d95ae instance has evacuate
2016-08-18 17:40:29,968 - compute - WARNING -181d7c2f-9aa3-4bd5-a3b1-15e2d5c20db7 instance has evacuate
```

云主机实现自动迁移, 本次过程中迁移了两台instance

* 查看自动迁移的云主机所在计算节点

```
# nova show 8e27d16f-cea8-4324-8fef-6511688d95ae
# nova show 181d7c2f-9aa3-4bd5-a3b1-15e2d5c20db7
```

发现两台云主机已经从compute节点node-24自动迁移至node-14节点, 自动迁移测试完成


`重要:目前云主机自动迁移支持云主机处于ACTIVE与SHUTOFF状态的云主机, ERROR状态的云主机也可以进行迁移, 但迁移时会检测, ERROR状态是否存在过ACTIVE运行(如果没有那么证明云主机创建时就出现了错误, 此时不能进行自动迁移)`
