### 常用管理命令
由上面简介， 除了 neutron 的部分服务，大部分 OpenStack 服务都是由 systemd 管理的，用 haproxy 来做负载均衡，对于这些大部分的服务，相关的管理命令为：
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

除此之外，一些和各个服务相关的命令显示在下面：

* [keystone](#keystone)
* [glance](#glance)
* [cinder](#cinder)
* [nova](#nova)
* [neutron](#neutron)
* [ceilometer](#ceilometer)

#### keystone

##### 服务管理
keystone只有一个服务：keystone，服务的管理如下:

```
# systemctl start openstack-keystone.service #启动服务
# systemctl stop openstack-keystone.service #关闭服务
# systemctl restart openstack-keystone.service #重启服务
```

##### 列出各个服务的 API 地址：
```
[root@node-1 ~]# keystone endpoint-list
+----------------------------------+-----------+---------------------------------------+-------------------------------------------+-------------------------------------------+----------------------------------+
|                id                |   region  |               publicurl               |                internalurl                |                  adminurl                 |            service_id            |
+----------------------------------+-----------+---------------------------------------+-------------------------------------------+-------------------------------------------+----------------------------------+
| 056375a459954f32a442feb1c25cb791 | RegionOne |  http://25.0.0.2:8773/services/Cloud  |  http://172.16.101.2:8773/services/Cloud  |  http://172.16.101.2:8773/services/Admin  | 89bb5e12c7844f78b30106e4ac66dfc8 |
| 08671aaca84c41cb9d0177c4e307c2ee | RegionOne | http://25.0.0.2:8776/v1/%(tenant_id)s | http://172.16.101.2:8776/v1/%(tenant_id)s | http://172.16.101.2:8776/v1/%(tenant_id)s | dfcc03186d014827be57cdfee1e9b39a |
| 0964d483057b490db376b91b4db690c1 | RegionOne |       http://25.0.0.2:5000/v2.0       |       http://172.16.101.2:5000/v2.0       |       http://172.16.101.2:35357/v2.0      | c67a0f60122e48a990413047cbf86e3e |
| 772e46eb4ee74255b5c59838200880f5 | RegionOne |          http://25.0.0.2:8777         |          http://172.16.101.2:8777         |          http://172.16.101.2:8777         | 31d82ea106e14a1eb208bacfa3d429bf |
| 92036ff4f9bc4c268bc329ef1ebb573d | RegionOne |         http://25.0.0.2:9696/         |         http://172.16.101.2:9696/         |         http://172.16.101.2:9696/         | 9149496e7e3d4df7bbc105ff649a681e |
| a1a338113fd24c459c660a67312769af | RegionOne | http://25.0.0.2:8776/v2/%(tenant_id)s | http://172.16.101.2:8776/v2/%(tenant_id)s | http://172.16.101.2:8776/v2/%(tenant_id)s | 3e5a0937bac543f9990305cde8d4ea77 |
| d18c99467746404b8a13971941351a16 | RegionOne | http://25.0.0.2:8774/v2/%(tenant_id)s | http://172.16.101.2:8774/v2/%(tenant_id)s | http://172.16.101.2:8774/v2/%(tenant_id)s | dd6c7d017edd4e5eaec2d73cbd05df98 |
| ec3ce0807cd04d83846d3f00d28b1021 | RegionOne |          http://25.0.0.2:9292         |          http://172.16.101.2:9292         |          http://172.16.101.2:9292         | ebe9e523e5974cefaed6ebc62b4d94e3 |
| f3f898e963db4cca85c16a0b02448604 | RegionOne | http://25.0.0.2:8004/v1/%(tenant_id)s | http://172.16.101.2:8004/v1/%(tenant_id)s | http://172.16.101.2:8004/v1/%(tenant_id)s | 8f7a2951a90f4c778d4adbae0e759a81 |
| f68316be515b45a2b478cbbbf4d6b7d4 | RegionOne |        http://25.0.0.2:8000/v1/       |        http://172.16.101.2:8000/v1/       |        http://172.16.101.2:8000/v1/       | 6f2735fa26f34cdfa9292fe6d1bb050b |
+----------------------------------+-----------+---------------------------------------+-------------------------------------------+-------------------------------------------+----------------------------------+

```

##### 刷新过期 token

目前 keystone 的 token 是存储在mysql数据库中，OpenStack 不会负责清理这些 token，如果长期使用，会发现用户登录 Horzion 登录变慢，mysql CPU 占用较高，Mysql 数据库变大。这时候要考虑清理 keystone 数据库中 token 表，所幸 keystone 提供了一个指令来清理这些过期 token。
```
# keystone-manage token_flush
```

> ###### 注意
> 目前 eayunstack 已经集成了脚本来自动清理过期 token。

#### glance
##### 服务管理
glance 有两个服务:

| 服务 | 服务名 | 运行的节点 |
| ---- | ---- | ---- |
| glance-api | openstack-glance-api.service | 控制节点 |
| glanceo-registry | openstack-glance-registry.service | 控制节点 |

服务管理如下：
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

#### cinder
##### 服务管理
cinder 有四个服务:

| 服务 | 服务名 | 运行的节点 |
| ---- | ---- | ---- |
| cinder-api | openstack-cinder-api.service | 控制节点 |
| cinder-scheduler | openstack-cinder-scheduler.service | 控制节点 |
| cinder-volume | openstack-cinder-volume.service | 控制节点 |
| cinder-backup | openstack-cinder-backup.service | 控制节点 |

服务管理如下：
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

##### 列出所有cinder服务和状态
```
[root@node-1 ~]# cinder service-list
+------------------+--------------------+------+----------+-------+----------------------------+-----------------+
|      Binary      |        Host        | Zone |  Status  | State |         Updated_at         | Disabled Reason |
+------------------+--------------------+------+----------+-------+----------------------------+-----------------+
| cinder-scheduler |       cinder       | nova | enabled  |   up  | 2015-04-28T03:18:34.000000 |       None      |
|  cinder-volume   |       cinder       | nova | disabled |  down | 2015-04-21T10:49:42.000000 |       None      |
|  cinder-volume   | cinder@cinder_ceph | nova | enabled  |   up  | 2015-04-28T03:18:38.000000 |       None      |
|  cinder-volume   | cinder@cinder_eqlx | nova | enabled  |   up  | 2015-04-28T03:18:32.000000 |       None      |
+------------------+--------------------+------+----------+-------+----------------------------+-----------------+
```

##### 列出目前支持的后端(目前仅支持ceph和eqlx)
```
[root@node-1 ~]# cinder type-list
+--------------------------------------+------+
|                  ID                  | Name |
+--------------------------------------+------+
| 585a2694-94da-438c-ad07-b1ba9d256ddd | rbd  |
| c2400021-3bd4-4fdc-b7ba-5eba502d7467 | eqlx |
+--------------------------------------+------+
```

> ###### 注意
> 关于 cinder-backup 服务的一些特殊说明:
>
> 由于该服务主要是为了备份 eqlx 存储，而且该服务不会自动备份 cinder volume，反而开启该服务会占用我们的 eqlx 的一个 session（eqlx最大session为7）。因此该服务按需开启，具体的备份请参考 FAQ相关章节

#### nova
##### 服务管理
| 服务 | 服务名 | 运行的节点 |
| ---- | ---- | ---- |
| nova-api | openstack-nova-api.service | 控制节点 |
| nova-cert | openstack-nova-cert.service | 控制节点 |
| nova-conductor | openstack-nova-conductor.service | 控制节点 |
| nova-consoleauth | openstack-nova-consoleauth.service | 控制节点 |
| nova-novncproxy | openstack-nova-novncproxy.service | 控制节点 |
| nova-objectstore | openstack-nova-objectstore.service | 控制节点 |
| nova-scheduler | openstack-nova-scheduler.service | 控制节点 |
| nova-compute | openstack-nova-compute.service | 计算节点 |

服务管理如下：
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

##### 查看所有运行nova服务的主机
```
[root@node-1 ~]# nova host-list
+-------------------+-------------+----------+
| host_name         | service     | zone     |
+-------------------+-------------+----------+
| node-1.domain.tld | consoleauth | internal |
| node-1.domain.tld | scheduler   | internal |
| node-1.domain.tld | conductor   | internal |
| node-1.domain.tld | cert        | internal |
| node-3.domain.tld | consoleauth | internal |
| node-3.domain.tld | scheduler   | internal |
| node-3.domain.tld | conductor   | internal |
| node-2.domain.tld | consoleauth | internal |
| node-2.domain.tld | scheduler   | internal |
| node-2.domain.tld | conductor   | internal |
| node-3.domain.tld | cert        | internal |
| node-2.domain.tld | cert        | internal |
| node-5.domain.tld | compute     | nova     |
| node-7.domain.tld | compute     | nova     |
+-------------------+-------------+----------+
```

##### 查看所有运行nova计算服务的主机
```
[root@node-1 ~]# nova hypervisor-list
+----+---------------------+
| ID | Hypervisor hostname |
+----+---------------------+
| 3  | node-5.domain.tld   |
| 6  | node-7.domain.tld   |
+----+---------------------+
```

##### 查看所有nova服务的状态
如果服务 **State** 为down，那么说明改服务异常，需要管理人员检查相应的环境
```
[root@node-1 ~]# nova service-list
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+
| Id | Binary           | Host              | Zone     | Status  | State | Updated_at                 | Disabled Reason |
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+
| 1  | nova-consoleauth | node-1.domain.tld | internal | enabled | up    | 2015-04-28T03:11:56.000000 | -               |
| 2  | nova-scheduler   | node-1.domain.tld | internal | enabled | up    | 2015-04-28T03:12:05.000000 | -               |
| 3  | nova-conductor   | node-1.domain.tld | internal | enabled | up    | 2015-04-28T03:11:57.000000 | -               |
| 4  | nova-cert        | node-1.domain.tld | internal | enabled | up    | 2015-04-28T03:11:58.000000 | -               |
| 6  | nova-consoleauth | node-3.domain.tld | internal | enabled | up    | 2015-04-28T03:11:56.000000 | -               |
| 9  | nova-scheduler   | node-3.domain.tld | internal | enabled | up    | 2015-04-28T03:11:58.000000 | -               |
| 12 | nova-conductor   | node-3.domain.tld | internal | enabled | up    | 2015-04-28T03:12:01.000000 | -               |
| 18 | nova-consoleauth | node-2.domain.tld | internal | enabled | up    | 2015-04-28T03:11:56.000000 | -               |
| 21 | nova-scheduler   | node-2.domain.tld | internal | enabled | up    | 2015-04-28T03:11:58.000000 | -               |
| 24 | nova-conductor   | node-2.domain.tld | internal | enabled | up    | 2015-04-28T03:12:02.000000 | -               |
| 30 | nova-cert        | node-3.domain.tld | internal | enabled | up    | 2015-04-28T03:12:00.000000 | -               |
| 33 | nova-cert        | node-2.domain.tld | internal | enabled | up    | 2015-04-28T03:12:01.000000 | -               |
| 36 | nova-compute     | node-5.domain.tld | nova     | enabled | up    | 2015-04-28T03:12:03.000000 | -               |
| 39 | nova-compute     | node-7.domain.tld | nova     | enabled | up    | 2015-04-28T03:12:01.000000 | None            |
+----+------------------+-------------------+----------+---------+-------+----------------------------+-----------------+
```

> ###### 注意
> 如果重启了计算节点的 libvirt，那么需要重启相应的计算节点的 nova-comupte 服务

#### neutron
##### 服务管理
| 服务/资源ID | 服务名/资源ID | 管理方式 |
| ---- | ---- | ---- |
| neutron-qos-agent | neutron-qos-agent.service | systemctl |
| neutron-server | neutron-server.service | systemctl |
| neutron-openvswitch-agent | p_neutron-openvswitch-agent | pacemaker |
| neutron-dhcp-agent | p_neutron-dhcp-agent | pacemaker |
| neutron-metadata-agent | p_neutron-metadata-agent | pacemaker |
| neutron-l3-agent | p_neutron-l3-agent | pacemaker |
|neutron-lbaas-agent | p_neutron-lbaas-agent | pacemaker |

使用 sytemctl 管理的服务：
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

使用 pacemaker 管理的服务见: [EayunStack 基础服务集群中的常用操作](../cluster_admin/ha_and_lb/eayunstack_cluster_operations.md)

##### 查看网络各个 agent 及其状态
```
[root@node-1 ~]# neutron agent-list
+--------------------------------------+--------------------+-------------------+-------+----------------+---------------------------+
| id                                   | agent_type         | host              | alive | admin_state_up | binary                    |
+--------------------------------------+--------------------+-------------------+-------+----------------+---------------------------+
| 0ac494c3-7948-4690-98e3-33b535ebcad1 | Metadata agent     | node-3.domain.tld | :-)   | True           | neutron-metadata-agent    |
| 1ae1aab8-68df-4fa8-bc38-58a16f0a130c | Open vSwitch agent | node-7.domain.tld | :-)   | True           | neutron-openvswitch-agent |
| 25fa1303-9e59-4340-869d-0756bda40e85 | Open vSwitch agent | node-3.domain.tld | :-)   | True           | neutron-openvswitch-agent |
| 4b3e887d-98e1-45a7-8677-d775cc7939c3 | DHCP agent         | node-1.domain.tld | :-)   | True           | neutron-dhcp-agent        |
| 4b624ab0-17cf-4fe6-9e63-c2df5ed384f6 | L3 agent           | node-3.domain.tld | :-)   | True           | neutron-l3-agent          |
| 4c90363c-213d-4932-a2ab-b6fd448b75b7 | Metadata agent     | node-1.domain.tld | :-)   | True           | neutron-metadata-agent    |
| a7114d66-2cac-4dfc-b54c-52520d0305d1 | Open vSwitch agent | node-1.domain.tld | :-)   | True           | neutron-openvswitch-agent |
| d3616e29-bcce-42e3-a08b-070947d825b0 | Open vSwitch agent | node-2.domain.tld | :-)   | True           | neutron-openvswitch-agent |
| e2f9bbeb-27b9-4bbc-82c5-7fd317855084 | Metadata agent     | node-2.domain.tld | :-)   | True           | neutron-metadata-agent    |
| e8a0996f-e335-4bd0-98ae-b92fa21410c5 | Open vSwitch agent | node-5.domain.tld | :-)   | True           | neutron-openvswitch-agent |
| e9630c91-0a91-45d8-804c-672ea451368e | L3 agent           | node-1.domain.tld | :-)   | True           | neutron-l3-agent          |
| fbcfb7a1-380d-4e17-ac91-aa0b6fa7f74d | L3 agent           | node-2.domain.tld | :-)   | True           | neutron-l3-agent          |
+--------------------------------------+--------------------+-------------------+-------+----------------+---------------------------+
```

#### horzion
horzion 依托于 apache 运行, 服务名为 httpd.service
```
# systemctl start httpd.service #启动服务
# systemctl stop httpd.service #关闭服务
# systemctl restart httpd.service #重启服务
```

#### ceilometer
ceilometer 服务分为systemctl 与pcs管理,ceilometer还包括提供数据库的mongo服务
#####　服务管理
| 服务/资源ID | 服务名/资源ID | 管理方式 | 运行节点|
| ---- | ---- | ---- | ---- |
|ceilometer-alarm-notifier|openstack-ceilometer-alarm-notifier.service|systemctl|控制节点|
|ceilometer-collector|openstack-ceilometer-collector.service|systemctl|控制节点|
|ceilometer-agent-notification|openstack-ceilometer-notification.service|systemctl|控制节点|
|ceilometer-api|httpd|systemctl|控制节点|
|ceilometer-agent-central|p_openstack-ceilometer-central|pacemaker|控制节点|
|ceilometer-alarm-evaluator|p_openstack-ceilometer-alarm-evaluator|pacemaker|控制节点|
|ceilometer-agent-compute|openstack-ceilometer-compute.service|systemctl|计算节点|
|mongod|mongod.service|systemctl|Mongo节点|

使用 systemctl 管理的服务:
```
# systemctl start [服务名] #启动服务
# systemctl stop [服务名] #关闭服务
# systemctl restart [服务名] #重启服务
```

使用 pacemaker 管理的服务见: [EayunStack 基础服务集群中的常用操作](../cluster_admin/ha_and_lb/eayunstack_cluster_operations.md)

举例查询一个内存使用查询:

```
[root@node-15 ~](controller)# ceilometer sample-list -m memory.usage -q "resource_id=ce9e8996-06ad-4833-a9d2-4fe382d1d42c" -l 10
+--------------------------------------+--------------+-------+---------------+------+---------------------+
| Resource ID                          | Name         | Type  | Volume        | Unit | Timestamp           |
+--------------------------------------+--------------+-------+---------------+------+---------------------+
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2220993042 | %    | 2016-06-14T02:27:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2191429138 | %    | 2016-06-14T02:26:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2220993042 | %    | 2016-06-14T02:25:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2250556946 | %    | 2016-06-14T02:24:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2220993042 | %    | 2016-06-14T02:23:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2250556946 | %    | 2016-06-14T02:22:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2250556946 | %    | 2016-06-14T02:21:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.228012085  | %    | 2016-06-14T02:20:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2250556946 | %    | 2016-06-14T02:19:24 |
| ce9e8996-06ad-4833-a9d2-4fe382d1d42c | memory.usage | gauge | 12.2220993042 | %    | 2016-06-14T02:18:24 |
+--------------------------------------+--------------+-------+---------------+------+---------------------+

```

注意：如果重启环境，要保证ceilometer使用的数据库Mongo节点必须在controller节点启动之前完成启动，Mongo节点为ceilometer提供数据库查询与数据存储服务.
