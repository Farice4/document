# OpenStack组件检测

该命令用于对EayunStack环境中各节点上运行的OpenStack组件进行检测。在不同角色的节点上执行该命令时所检测的组件及检测内容不同，具体见下表：

|节点角色|检测组件|检测内容|
|----|----|----|
|controller|Keystone<br />Glance<br />Nova<br />Neutron<br />Cinder|配置文件正确性检测<br />服务运行状态检测<br />服务与数据库的连通性检测<br />服务可用性检测|
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
[fuel]# eayunstack --debug doctor stack --controller
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ DEBUG ] (controller) (node-6.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-6.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-6.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-6.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ DEBUG ] (controller) (node-8.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-8.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-8.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-8.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
```

* Controller节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[controller]# eayunstack --debug doctor stack --controller
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
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
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.9  ***************
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): -Service Status
...

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.7  ***************
[ DEBUG ] =====> start running check_compute_profile 
[ INFO  ] Checking "Nova" Component
...
[ DEBUG ] =====> start running check_compute_service 
[ INFO  ] Checking "Nova" Component
[ INFO  ] -Service Status
...

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.12 ***************
[ DEBUG ] (compute) (node-10.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-10.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-10.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-10.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-10.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-10.eayun.com): -Service Status
...
```

* Compute节点执行命令

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[compute]# eayunstack --debug doctor stack --compute
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): -Service Status
...
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
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: mongo      Node: 172.16.100.11 ***************
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_profile 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): Profile: /etc/mongodb.conf
...
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_service 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): -Service Status
...

```

* Mongo节点执行命令

```
[mongo]# eayunstack --debug doctor stack --mongo
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_profile 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): Profile: /etc/mongodb.conf
...
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_service 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): -Service Status
...
```

## 单独检测配置文正确性

该命令可以单独检测配置文件的正确性，例如只检测Controller节点上各组件的配置文件。

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[controller]# eayunstack --debug doctor stack --controller --profile
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/cinder/cinder.conf
...
```

## 单独检测服务运行状态

该命令可以单独检测服务运行状态，例如只检测Controller节点上各组件服务的运行状态。

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[controller]# eayunstack --debug doctor stack --controller --service
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
```

## 检测所有检测对象

> ###### 注意
>
> 该命令需要在Fuel节点上执行

> **注意**
>
> 以下示例中省略掉了部分Debug信息

```
[fuel]# eayunstack --debug doctor stack -a
[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.8  ***************
[ DEBUG ] (controller) (node-6.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-6.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-6.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-6.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-6.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-6.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-6.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-6.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-6.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-6.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-6.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-6.eayun.com): Check Successfully.

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.10 ***************
[ DEBUG ] (controller) (node-8.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-8.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-8.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-8.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-8.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-8.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-8.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-8.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-8.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-8.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-8.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-8.eayun.com): Check Successfully.

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: controller Node: 172.16.100.6  ***************
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_profile 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/keystone/keystone.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-api.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/glance/glance-registry.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/nova/nova.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/neutron/neutron.conf
...
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): Profile: /etc/cinder/cinder.conf
...
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_controller_service 
[ INFO  ] (controller) (node-5.eayun.com): Checking "Keystone" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Glance" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Nova" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Neutron" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.
[ INFO  ] (controller) (node-5.eayun.com): Checking "Cinder" Component
[ DEBUG ] (controller) (node-5.eayun.com): -Service Status
...
[ DEBUG ] (controller) (node-5.eayun.com): -DB Connectivity
[ DEBUG ] (controller) (node-5.eayun.com): Check Sucessfully.
[ DEBUG ] (controller) (node-5.eayun.com): -Service Availability
[ DEBUG ] (controller) (node-5.eayun.com): Check Successfully.

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.9  ***************
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-7.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-7.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-7.eayun.com): -Service Status
...

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.7  ***************
[ DEBUG ] =====> start running check_compute_profile 
[ INFO  ] Checking "Nova" Component
...
[ DEBUG ] =====> start running check_compute_service 
[ INFO  ] Checking "Nova" Component
[ INFO  ] -Service Status
...

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: compute    Node: 172.16.100.12 ***************
[ DEBUG ] (compute) (node-10.eayun.com): =====> start running check_compute_profile 
[ INFO  ] (compute) (node-10.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-10.eayun.com): Profile: /etc/nova/nova.conf
...
[ DEBUG ] (compute) (node-10.eayun.com): =====> start running check_compute_service 
[ INFO  ] (compute) (node-10.eayun.com): Checking "Nova" Component
[ DEBUG ] (compute) (node-10.eayun.com): -Service Status
...

[ INFO  ] (fule) (fuel.domain.tld): *************** Role: mongo      Node: 172.16.100.11 ***************
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_profile 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): Profile: /etc/mongodb.conf
...
[ DEBUG ] (mongo) (node-9.eayun.com): =====> start running check_mongo_service 
[ INFO  ] (mongo) (node-9.eayun.com): Checking "Mongo" Component
[ DEBUG ] (mongo) (node-9.eayun.com): -Service Status
```









