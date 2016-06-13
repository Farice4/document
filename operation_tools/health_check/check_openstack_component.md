# OpenStack组件检测

该命令用于对EayunStack环境中各节点上运行的OpenStack组件进行检测。在不同角色的节点上执行该命令时所检测的组件及检测内容不同，具体见下表：

|节点角色|检测组件|检测内容|
|----|----|----|
|controller|Keystone<br />Glance<br />Nova<br />Neutron<br />Cinder<br />Ceilometer|配置文件正确性检测<br />服务运行状态检测<br />服务与数据库的连通性检测<br />服务可用性检测|
|compute|Nova|配置文件正确性检测<br />服务运行状态检测|
|mongo|Mongo|配置文件正确性检测<br />服务运行状态检测|

## 命令格式

```
# eayunstack doctor stack --help
usage: eayunstack doctor stack [-h] [--profile] [--service] [--controller]
                               [--compute] [--mongo] [-a]

Check OpenStack Compent

optional arguments:
  -h, --help    show this help message and exit
  --profile     Check Profile
  --service     Check Service Status
  --controller  Check All Controller Node
  --compute     Check All Compute Node
  --mongo       Check All Mongo Node
  -a, --all     Check ALL
```

## Controller节点检测

> ###### 注意
>
> 该命令可以在**Fuel节点**或**Controller节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Controller节点进行检测。在**Controller节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
(fuel)# eayunstack --debug doctor stack --controller
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ DEBUG ] (controller) (node-17.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-17.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-17.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-17.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-17.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-17.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ INFO  ] (controller) (node-17.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-17.eayun.com): Profile: /etc/ceilometer/ceilometer.conf
...
[ DEBUG ] (controller) (node-17.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-17.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-keystone is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-keystone is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-17.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-glance-api is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-glance-api is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-glance-registry is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-glance-registry is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-17.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-api is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-api is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-conductor is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-conductor is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-scheduler is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-nova-scheduler is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-17.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-server is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-server is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-ovs-cleanup is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-ovs-cleanup is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openvswitch is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openvswitch is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-metering-agent is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service neutron-metering-agent is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-17.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-api is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-api is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-scheduler is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-scheduler is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-volume is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-cinder-volume is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-17.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-17.eayun.com): -Service Status
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-alarm-notifier is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-alarm-notifier is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-collector is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-collector is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-notification is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service openstack-ceilometer-notification is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): Service httpd is running ...
[ DEBUG ] (controller) (node-17.eayun.com): Service httpd is enabled ...
[ DEBUG ] (controller) (node-17.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-17.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-17.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-17.eayun.com): Check Successfully.
[ DEBUG ] (controller) (node-18.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-18.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-18.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-18.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-18.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-18.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ INFO  ] (controller) (node-18.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-18.eayun.com): Profile: /etc/ceilometer/ceilometer.conf
...
[ DEBUG ] (controller) (node-18.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-18.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-keystone is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-keystone is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-18.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-glance-api is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-glance-api is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-glance-registry is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-glance-registry is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-18.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-api is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-api is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-conductor is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-conductor is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-scheduler is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-nova-scheduler is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-18.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-server is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-server is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-ovs-cleanup is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-ovs-cleanup is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openvswitch is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openvswitch is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-metering-agent is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service neutron-metering-agent is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-18.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-api is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-api is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-scheduler is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-scheduler is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-volume is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-cinder-volume is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-18.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-18.eayun.com): -Service Status
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-alarm-notifier is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-alarm-notifier is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-collector is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-collector is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-notification is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service openstack-ceilometer-notification is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): Service httpd is running ...
[ DEBUG ] (controller) (node-18.eayun.com): Service httpd is enabled ...
[ DEBUG ] (controller) (node-18.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-18.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-18.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-18.eayun.com): Check Successfully.
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/ceilometer/ceilometer.conf
...
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
```

* Controller节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[controller]# eayunstack --debug doctor stack --controller
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/ceilometer/ceilometer.conf
...
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
```

## Compute节点检测

> ###### 注意
>
> 该命令可以在**Fuel节点**或**Compute节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Compute节点进行检测。在**Compute节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[fuel]# eayunstack --debug doctor stack --compute
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-24.eayun.com (compute   ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-14.eayun.com (compute   ) ==>
[ DEBUG ] (compute) (node-24.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-24.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-24.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-24.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-24.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-24.eayun.com): -Service Status
[ ERROR ] (compute) (node-24.eayun.com): Service openstack-nova-compute is not running ...
[ DEBUG ] (compute) (node-24.eayun.com): Service openstack-nova-compute is enabled ...
[ DEBUG ] (compute) (node-24.eayun.com): Service openstack-ceilometer-compute is running ...
[ DEBUG ] (compute) (node-24.eayun.com): Service openstack-ceilometer-compute is enabled ...
[ DEBUG ] (compute) (node-14.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-14.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-14.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-14.eayun.com): -Service Status
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-nova-compute is running ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-nova-compute is enabled ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-ceilometer-compute is running ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-ceilometer-compute is enabled ...
```

* Compute节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[compute]# eayunstack --debug doctor stack --compute
[ DEBUG ] (compute) (node-14.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-14.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-14.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-14.eayun.com): -Service Status
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-nova-compute is running ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-nova-compute is enabled ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-ceilometer-compute is running ...
[ DEBUG ] (compute) (node-14.eayun.com): Service openstack-ceilometer-compute is enabled ...
```

## Mongo节点检测

> ###### 注意
> 该命令可以在**Fuel节点**或**Mongo节点**上运行。在**Fuel节点**上运行该命令，可以同时对所有Mongo节点进行检测。在**Mongo节点**上运行该命令，只检测当前节点。

* Fuel节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[fuel]# eayunstack --debug doctor stack --mongo
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-22.eayun.com (mongo     ) ==>
[ DEBUG ] (mongo) (node-22.eayun.com): =====> start running check_mongo_profile 
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-22.eayun.com): Profile: /etc/mongodb.conf
...
[ DEBUG ] (mongo) (node-22.eayun.com): =====> start running check_mongo_service 
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-22.eayun.com): -Service Status
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is running ...
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is enabled ...
```

* Mongo节点执行命令

```
[mongo]# eayunstack --debug doctor stack --mongo
[ DEBUG ] (mongo) (node-22.eayun.com): =====> start running check_mongo_profile 
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-22.eayun.com): Profile: /etc/mongodb.conf
...
[ DEBUG ] (mongo) (node-22.eayun.com): =====> start running check_mongo_service 
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-22.eayun.com): -Service Status
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is running ...
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is enabled ...
```

## 单独检测配置文正确性

该命令可以单独检测配置文件的正确性，例如只检测Controller节点上各组件的配置文件。

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[controller]# eayunstack --debug doctor stack --controller --profile
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): Profile: /etc/ceilometer/ceilometer.conf
...
```

## 单独检测服务运行状态

该命令可以单独检测服务运行状态，例如只检测Controller节点上各组件服务的运行状态。

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-keystone is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-glance-registry is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-conductor is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-nova-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-server is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-ovs-cleanup is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openvswitch is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service neutron-metering-agent is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-api is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-scheduler is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-cinder-volume is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ DEBUG ] (controller) (node-15.eayun.com): -Service Status
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-alarm-notifier is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-collector is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service openstack-ceilometer-notification is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service httpd is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-15.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-15.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-15.eayun.com): Check Successfully.
```

## 检测所有检测对象

> ###### 注意
>
> 该命令需要在Fuel节点上执行
>
> 如果需要输出Debug级别信息，可以使用"--debug"选项。

```
(fuel)# eayunstack doctor all
[ INFO  ] (fuel) (fuel.domain.tld): +++++++++++++ Check Basic Environment     +++++++++++++
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-24.eayun.com (compute   ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-14.eayun.com (compute   ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-22.eayun.com (mongo     ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-20.eayun.com (ceph-osd  ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-21.eayun.com (ceph-osd  ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-16.eayun.com (ceph-osd  ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): +++++++++++++ Check Cluster Environment   +++++++++++++
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-17.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-17.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-17.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-17.eayun.com): Ceph cluster check successfully !
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
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-17.eayun.com): The ceph space is used: 22.59%
[ INFO  ] (controller) (node-17.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-18.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-18.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-18.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-18.eayun.com): Ceph cluster check successfully !
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
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-18.eayun.com): The ceph space is used: 22.59%
[ INFO  ] (controller) (node-18.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking rabbitmq cluster status
[ INFO  ] (controller) (node-15.eayun.com): Rabbitmq cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking mysql cluster status
[ INFO  ] (controller) (node-15.eayun.com): Mysql cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking haproxy cluster status
[ INFO  ] (controller) (node-15.eayun.com): Haproxy cluster check successfully !
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph cluster status
[ INFO  ] (controller) (node-15.eayun.com): Ceph cluster check successfully !
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
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking ceph space
[ INFO  ] (controller) (node-15.eayun.com): The ceph space is used: 22.59%
[ INFO  ] (controller) (node-15.eayun.com): =====> Checking HAProxy resource status
[ INFO  ] (fuel) (fuel.domain.tld): +++++++++++++ Check OpenStack Environment +++++++++++++
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-17.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-18.eayun.com (controller) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-15.eayun.com (controller) ==>
[ INFO  ] (controller) (node-17.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Ceilometer" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-17.eayun.com): Checking "Ceilometer" Component
[ ERROR ] (controller) (node-17.eayun.com): Service nova-compute on node-24.eayun.com state is down
[ INFO  ] (controller) (node-18.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Ceilometer" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-18.eayun.com): Checking "Ceilometer" Component
[ ERROR ] (controller) (node-18.eayun.com): Service nova-compute on node-24.eayun.com state is down
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Keystone" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Glance" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Nova" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Neutron" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Cinder" Component
[ INFO  ] (controller) (node-15.eayun.com): Checking "Ceilometer" Component
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-24.eayun.com (compute   ) ==>
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-14.eayun.com (compute   ) ==>
[ INFO  ] (compute) (node-24.eayun.com): Checking "Nova" Component
[ INFO  ] (compute) (node-24.eayun.com): Checking "Nova" Component
[ ERROR ] (compute) (node-24.eayun.com): Service openstack-nova-compute is not running ...
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ INFO  ] (compute) (node-14.eayun.com): Checking "Nova" Component
[ INFO  ] (fuel) (fuel.domain.tld): <== Push check cmd to node-22.eayun.com (mongo     ) ==>
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
[ INFO  ] (mongo) (node-22.eayun.com): Checking "Mongo" Component
```