# 集群状态检测

该命令用于对EayunStack环境中的集群进行检测，包括对MySQL，RabbitMQ，Ceph，Haproxy，Pacemaker集群的检测。

## 命令格式

```
# eayunstack doctor cls --help
usage: eayunstack doctor cls [-h]
                             [-n {mysql,rabbitmq,rabbitmqqueues,ceph,haproxy,pacemaker,cephspace,haproxyresource}]
                             [-a]

Check cluster

optional arguments:
  -h, --help            show this help message and exit
  -n {mysql,rabbitmq,rabbitmqqueues,ceph,haproxy,pacemaker,cephspace,haproxyresource}
                        Cluster Name
  -a, --all             Check ALL
```

## MySQL集群检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n mysql
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-17.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-18.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-15.eayun.com): Mysql cluster check successfully !
```

* Controller节点执行命令

```
[controller]$ eayunstack doctor cls -n mysql
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-15.eayun.com): Mysql cluster check successfully !
```

## RabbitMQ集群检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n rabbitmq
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-17.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-18.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-15.eayun.com): Rabbitmq cluster check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -n rabbitmq
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-15.eayun.com): Rabbitmq cluster check successfully !
```

## Rabbitmq 队列状态检测

* Fuel节点执行命令

```
(fuel)# eayunstack --debug doctor cls -n rabbitmqqueues
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-17.eayun.com): Queue lma_notifications.info has 166680 messages and has been used 191 MB memory.
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-18.eayun.com): Queue lma_notifications.info has 166680 messages and has been used 191 MB memory.
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-15.eayun.com): Queue lma_notifications.info has 166680 messages and has been used 191 MB memory.
```

* Controller节点执行命令

```
(controller)# eayunstack --debug doctor cls -n rabbitmqqueues
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-15.eayun.com): Queue lma_notifications.info has 166685 messages and has been used 141 MB memory.
```

## Ceph集群检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**或**Ceph-osd节点**上运行。在**Fuel节点**上运行该命令，会随机连接一个controller节点进行检测。在**Controller节点**或**Ceph-osd节**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n ceph
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-6.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-6.eayun.com): Ceph osd status check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -n ceph
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-15.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-15.eayun.com): Ceph osd status check successfully !
```

* Ceph-osd节点执行命令

```
[ceph-osd]# eayunstack --debug doctor cls -n ceph
[ INFO  ] =====> Checking ceph cluster status
[ INFO  ] Ceph cluster check successfully !
[ INFO  ] =====> Checking ceph osd status
[ INFO  ] Ceph osd status check successfully !
```

## Pacemaker

> ###### 注意
> 该命令仅可以再**Controller节点**上运行。

* Controller节点执行命令

```
(controller)# eayunstack doctor cls -n pacemaker
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource managed status
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-17.eayun.com
```

## Haproxy

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n haproxy
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-17.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-18.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-15.eayun.com): Haproxy cluster check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -n haproxy
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-15.eayun.com): Haproxy cluster check successfully !
```

## Ceph使用空间检测

```
(controller)# eayunstack --debug doctor cls -n cephspace
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-15.eayun.com): The ceph space is used: 22.6%
```

## HAProxy资源状态检测

```
(controller)# eayunstack doctor cls -n haproxyresource
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (controller) (node-15.eayun.com): Haproxyresource cluster check successfully!
[root@node-15 ~](controller)# eayunstack --debug doctor cls -n haproxyresource
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking HAProxy resource status
[ DEBUG ] (controller) (node-15.eayun.com): Stats on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): Stats on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): horizon on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): horizon on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): horizon on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): horizon on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): horizon on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-1 on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-1 on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-1 on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-1 on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-1 on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-2 on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-2 on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-2 on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-2 on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): keystone-2 on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-1 on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-1 on node-15 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-1 on node-17 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-1 on node-18 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-1 on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-2 on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-2 on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-2 on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-2 on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-api-2 on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): nova-metadata-api on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): nova-metadata-api on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-metadata-api on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-metadata-api on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-metadata-api on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): cinder-api on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): cinder-api on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): cinder-api on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): cinder-api on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): cinder-api on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): glance-api on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): glance-api on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-api on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-api on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-api on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): neutron on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): neutron on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): neutron on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): neutron on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): neutron on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): glance-registry on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): glance-registry on node-15 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-registry on node-17 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-registry on node-18 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): glance-registry on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): rabbitmq on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): rabbitmq on node-15 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): rabbitmq on node-17 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): rabbitmq on node-18 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): rabbitmq on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): mysqld on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): mysqld on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): mysqld on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): mysqld on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): mysqld on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): ceilometer on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): ceilometer on node-15 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): ceilometer on node-17 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): ceilometer on node-18 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): ceilometer on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cfn on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cfn on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cfn on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cfn on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cfn on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cloudwatch on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cloudwatch on node-15 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cloudwatch on node-17 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cloudwatch on node-18 status is UP, check_status is L7OK.
[ DEBUG ] (controller) (node-15.eayun.com): heat-api-cloudwatch on BACKEND status is UP.
[ DEBUG ] (controller) (node-15.eayun.com): nova-novncproxy on FRONTEND status is OPEN.
[ DEBUG ] (controller) (node-15.eayun.com): nova-novncproxy on node-15 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-novncproxy on node-17 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-novncproxy on node-18 status is UP, check_status is L4OK.
[ DEBUG ] (controller) (node-15.eayun.com): nova-novncproxy on BACKEND status is UP.
[ INFO  ] (controller) (node-15.eayun.com): Haproxyresource cluster check successfully!
```

## 检测所有检测对象

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
(fuel)# eayunstack doctor cls -a
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-17.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq managed status
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-17.eayun.com): Queue lma_notifications.info has 166744 messages and has been used 191 MB memory.
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-17.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-17.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking ceph cluster status
[ WARNIN] (controller) (node-17.eayun.com): Ceph cluster check faild !
[ WARNIN] (controller) (node-17.eayun.com): HEALTH_WARN noout flag(s) set
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-17.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-17.eayun.com): Resource vip__public check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking pacemaker resource managed status
[ INFO  ] (controller) (node-17.eayun.com): Resource vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource vip__management was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-ceilometer-central was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-dhcp-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource ping_vip__public was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource ping_vip__public was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource ping_vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-heat-engine was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-heat-engine was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_openstack-heat-engine was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-metadata-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-metadata-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-metadata-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-l3-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-l3-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-l3-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_mysql was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_mysql was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_mysql was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_haproxy was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_haproxy was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_haproxy was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-17.eayun.com): The ceph space is used: 19.68%
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-18.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq managed status
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-18.eayun.com): Queue lma_notifications.info has 166744 messages and has been used 191 MB memory.
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-18.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-18.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking ceph cluster status
[ WARNIN] (controller) (node-18.eayun.com): Ceph cluster check faild !
[ WARNIN] (controller) (node-18.eayun.com): HEALTH_WARN noout flag(s) set
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-18.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-18.eayun.com): Resource vip__public check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking pacemaker resource managed status
[ INFO  ] (controller) (node-18.eayun.com): Resource vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource vip__management was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-ceilometer-central was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-dhcp-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource ping_vip__public was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource ping_vip__public was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource ping_vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-heat-engine was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-heat-engine was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_openstack-heat-engine was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-metadata-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-metadata-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-metadata-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-l3-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-l3-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-l3-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_mysql was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_mysql was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_mysql was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_haproxy was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_haproxy was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_haproxy was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-18.eayun.com): The ceph space is used: 19.68%
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-15.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq managed status
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-15.eayun.com): Queue lma_notifications.info has 166744 messages and has been used 191 MB memory.
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-15.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-15.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph cluster status
[ WARNIN] (controller) (node-15.eayun.com): Ceph cluster check faild !
[ WARNIN] (controller) (node-15.eayun.com): HEALTH_WARN noout flag(s) set
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-15.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource managed status
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-15.eayun.com): The ceph space is used: 19.68%
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking HAProxy resource status
```

* Controller节点执行命令

```
(controller)# eayunstack doctor cls -a
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-15.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq managed status
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq queues status
[ WARNIN] (controller) (node-15.eayun.com): Queue lma_notifications.info has 166755 messages and has been used 141 MB memory.
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-15.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-15.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph cluster status
[ WARNIN] (controller) (node-15.eayun.com): Ceph cluster check faild !
[ WARNIN] (controller) (node-15.eayun.com): HEALTH_WARN noout flag(s) set
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-15.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking pacemaker resource managed status
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource vip__management was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-central was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-dhcp-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource ping_vip__public was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_openstack-heat-engine was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-openvswitch-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-metadata-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-l3-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_mysql was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_rabbitmq-server was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_haproxy was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-18.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): Resource p_neutron-lbaas-agent was managed on node node-17.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-15.eayun.com): The ceph space is used: 19.68%
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking HAProxy resource status
```