# 配置网络流量监控 #
Ceilometer 流量监控分为libvirt监控云主机内部网卡流量与neutron metering bandwidth采集经过路由的所有流量.本文主要讲解采用Neutron Metering Bandwidth 方式统计流量的配置与流量监控查询．

工作原理

> Bandwidth 流量采集，启用neutron-metering-agent服务，配置两个路由标签(一个用于网络流入，一个用于网络流出)，每个标签对应租户的私有网络地址，租户的地址经过路由，被iptables统计流入与流出的流量，neutron-metering-agent根据一定的时间进行统计与发布采集数据，最终由ceilometer获取并存储进数据库．
　　
> 同一子网下虚拟机互相访问不会统计，但不同子网间进过虚拟路由器的流量会被统计，虚拟机与外部网络互访也会统计．

#### Neutron Meter采集配置 ####

* 安装neutron-metering-agent服务，采用yum 方式直接安装
* 配置metering_agent.ini文件如下：

```
[DEFAULT]
# Show debugging output in log (sets DEBUG log level output)
debug = True

# Default driver:
# driver = neutron.services.metering.drivers.noop.noop_driver.NoopMeteringDriver
# Example of non-default driver
driver = neutron.services.metering.drivers.iptables.iptables_driver.IptablesMeteringDriver

# Interval between two metering measures
measure_interval = 30

# Interval between two metering reports
report_interval = 58

# interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver

use_namespaces = True

#interface_driver = neutron.agent.linux.interface.BridgeInterfaceDriver

```

备注：上面的measure_interval为采集间隔时间，report_interval为采集通告时间，它们的时间间隔都与上次采集与通告时间相关联．

* 启动neutron-metering-agent.service服务

* 配置neutron metering 采集标签

```
创建网络流量流入标签
[root@node-17 ~](controller)# neutron meter-label-create fabian-in --tenant-id=0bdd890d46c94ced8f35bd2af125b7a6
Created a new metering_label:
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| description |                                      |
| id          | 504ca435-97f0-47e8-97cf-a7b92b310fbe |
| name        | fabian-in                            |
| shared      | False                                |
| tenant_id   | 0bdd890d46c94ced8f35bd2af125b7a6     |
+-------------+--------------------------------------+

创建网络流量流出标签
[root@node-17 ~](controller)# neutron meter-label-create fabian-out --tenant-id=0bdd890d46c94ced8f35bd2af125b7a6
Created a new metering_label:
+-------------+--------------------------------------+
| Field       | Value                                |
+-------------+--------------------------------------+
| description |                                      |
| id          | ec1db1f0-6a86-4b4f-b28f-3076b17fc331 |
| name        | fabian-out                           |
| shared      | False                                |
| tenant_id   | 0bdd890d46c94ced8f35bd2af125b7a6     |
+-------------+--------------------------------------+

备注：其中--tenant-id 即租户id,创建的两个标签用于统计该租户的网络流量

```

*　配置neutron metering 采集规则(该规则创建必须存在以创建的采集标签)

```
创建网络流量流入规则
[root@node-17 ~](controller)# neutron meter-label-rule-create fabian-in 7.7.7.0/24 --direction  ingress
Created a new metering_label_rule:
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| direction         | ingress                              |
| excluded          | False                                |
| id                | c2f5f724-df0c-4076-b72b-6bbaa1db36d2 |
| metering_label_id | 504ca435-97f0-47e8-97cf-a7b92b310fbe |
| remote_ip_prefix  | 7.7.7.0/24                           |
+-------------------+--------------------------------------+

创建网络流量流出规则
[root@node-17 ~](controller)# neutron meter-label-rule-create fabian-out 7.7.7.0/24 --direction  egress
Created a new metering_label_rule:
+-------------------+--------------------------------------+
| Field             | Value                                |
+-------------------+--------------------------------------+
| direction         | egress                               |
| excluded          | False                                |
| id                | 14fc9c1b-40f3-412f-9137-b129d98d9bf6 |
| metering_label_id | ec1db1f0-6a86-4b4f-b28f-3076b17fc331 |
| remote_ip_prefix  | 7.7.7.0/24                           |
+-------------------+--------------------------------------+

```

* 查看已创建的meter标签

```
[root@node-17 ~](controller)# neutron meter-label-list
+--------------------------------------+------------------+-------------------+--------+
| id                                   | name             | description       | shared |
+--------------------------------------+------------------+-------------------+--------+
| ec1db1f0-6a86-4b4f-b28f-3076b17fc331 | fabian-out       |                   | False  |
| 2e102c61-3f15-4837-bc9e-bb531ce3799c | test-meter-issue |                   | False  |
| 504ca435-97f0-47e8-97cf-a7b92b310fbe | fabian-in        |                   | False  |
| 056423a2-1075-4937-bf29-6695a566108d | coffee-in        |                   | False  |
| b7636446-45cb-4a4b-b003-472a4b980eed | ct-test          |                   | False  |
| bb3c4264-d34b-488a-b7ad-34152e3d9c09 | ct-test          |                   | False  |
| c56f17e0-17bf-408f-b78b-aa6efb7df3d3 | zc_project       | zhao chao project | False  |
| edc71b82-e38a-4064-9107-cfa535218f0d | coffee-out       |                   | False  |
| fee4fe70-a614-4b06-9171-36231f97031d | coffee           |                   | False  |
+--------------------------------------+------------------+-------------------+--------+

```

*　查看已创建的meter rule规则

```
[root@node-17 ~](controller)# neutron meter-label-rule-list
+--------------------------------------+----------+-----------+------------------+
| id                                   | excluded | direction | remote_ip_prefix |
+--------------------------------------+----------+-----------+------------------+
| 14fc9c1b-40f3-412f-9137-b129d98d9bf6 | False    | egress    | 7.7.7.0/24       |
| 765f2a13-42d6-4db7-af28-feaf1ba895bf | False    | ingress   | 192.168.150.0/24 |
| c2f5f724-df0c-4076-b72b-6bbaa1db36d2 | False    | ingress   | 7.7.7.0/24       |
+--------------------------------------+----------+-----------+------------------+
备注:其中7.7.7.0/24为创建的测试标签规则
```

*　删除已创建的meter rule规则

```
[root@node-17 ~](controller)# neutron meter-label-rule-delete 14fc9c1b-40f3-412f-9137-b129d98d9bf6
Deleted metering_label_rule: 14fc9c1b-40f3-412f-9137-b129d98d9bf6

```

* 删除已创建的meter label标签

```
[root@node-17 ~](controller)# neutron meter-label-delete ec1db1f0-6a86-4b4f-b28f-3076b17fc331     
Deleted metering_label: ec1db1f0-6a86-4b4f-b28f-3076b17fc331

```

