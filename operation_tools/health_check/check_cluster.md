# 集群状态检测

该命令用于对EayunStack环境中的集群进行检测，包括对MySQL，RabbitMQ，Ceph，Haproxy，Pacemaker集群的检测。

## 命令格式

```
$ eayunstack doctor cls --help
eayunstack doctor cls --help
usage: eayunstack doctor cls [-h] [-n {mysql,rabbitmq,ceph,haproxy,pacemaker}]
                             [-a]

Check cluster

optional arguments:
  -h, --help            show this help message and exit
  -n {mysql,rabbitmq,ceph,haproxy,pacemaker}
                        Cluster Name
  -a, --all             Check ALL
```

## MySQL集群检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n mysql
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-6.eayun.com): Mysql cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-8.eayun.com): Mysql cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-5.eayun.com): Mysql cluster check successfully !
```

* Controller节点执行命令

```
[controller]$ eayunstack doctor cls -n mysql
# eayunstack --debug doctor cls -n mysql
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-5.eayun.com): Mysql cluster check successfully !
```

## RabbitMQ集群检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n rabbitmq
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-6.eayun.com): Rabbitmq cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-8.eayun.com): Rabbitmq cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-5.eayun.com): Rabbitmq cluster check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -n rabbitmq
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-5.eayun.com): Rabbitmq cluster check successfully !
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
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-5.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-5.eayun.com): Ceph osd status check successfully !
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
[controller]# eayunstack --debug doctor cls -n pacemaker
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-5.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__public check successfully !
```

## Haproxy

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -n haproxy
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-6.eayun.com): Haproxy cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-8.eayun.com): Haproxy cluster check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-5.eayun.com): Haproxy cluster check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -n haproxy
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-5.eayun.com): Haproxy cluster check successfully !
```

## 检测所有检测对象

> ###### 注意
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

```
[fuel]# eayunstack --debug doctor cls -a
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-6.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-6.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-6.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-6.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-6.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-6.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-6.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-6.eayun.com): Resource vip__public check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-8.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-8.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-8.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-8.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-8.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-8.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-8.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-8.eayun.com): Resource vip__public check successfully !

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-5.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-5.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-5.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-5.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-5.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-5.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__public check successfully !
```

* Controller节点执行命令

```
[controller]# eayunstack --debug doctor cls -a
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-5.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-5.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-5.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-5.eayun.com): Ceph cluster check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking ceph osd status
[ INFO  ] (controller) (node-5.eayun.com): Ceph osd status check successfully !
[ INFO  ] (controller) (node-5.eayun.com): =====> Checking pacemaker resource status
[ INFO  ] (controller) (node-5.eayun.com): Resource p_neutron-dhcp-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_ping_vip__public check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__management check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-alarm-evaluator check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_haproxy check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-openvswitch-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_openstack-heat-engine check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-lbaas-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-metadata-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_rabbitmq-server check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_neutron-l3-agent check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource p_openstack-ceilometer-central check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource clone_p_mysql check successfully !
[ INFO  ] (controller) (node-5.eayun.com): Resource vip__public check successfully !
```




















