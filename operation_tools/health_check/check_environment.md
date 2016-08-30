# 基础环境检测

该命令用于对EayunStack环境中的各节点进行基础环境检测，包括对NTP、Mongodb、Rabbitmq、SELinux、Daemon、Disk、Memory、Network、CPU、CPU负载的检测。

## 命令格式

```
# eayunstack doctor env --help
usage: eayunstack doctor env [-h]
                             [-n {ntp,mongodb,rabbitmqrestart,selinux,daemon,disk,memory,network,cpu,cpuload}]
                             [-a]

Check Environment Object

optional arguments:
  -h, --help            show this help message and exit
  -n {ntp,mongodb,rabbitmqrestart,selinux,daemon,disk,memory,network,cpu,cpuload}
                        Object Name
  -a, --all             Check ALL
```

> **注意**
>
> 在使用该命令时，如需输出Debug消息，请在"eayunstack"命令后使用"--debug"选项，如以下示例。


## NTP检测

该命令用于检测当前节点NTP服务运行状态及与上游NTP服务器的同步状态，当执行该命令输出类似如下信息时，当前节点NTP服务运行状态及与上游NTP服务器同步状态正常。

```
# eayunstack --debug doctor env -n ntp
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_ntp 
[ DEBUG ] (controller) (node-5.eayun.com): Service ntpd is running ...
[ DEBUG ] (controller) (node-5.eayun.com): Service ntpd is enabled ...
[ DEBUG ] (controller) (node-5.eayun.com): ntpserver is 172.16.100.2
```

## Mongodb检测

> 注意
>
> Mongodb服务只运行在mongo节点。

```
(mongo)# eayunstack --debug doctor env -n mongodb
[ DEBUG ] (mongo) (node-22.eayun.com): =====> start running check_mongodb 
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is running ...
[ DEBUG ] (mongo) (node-22.eayun.com): Service mongod is enabled ...
[ DEBUG ] (mongo) (node-22.eayun.com): mongod service is ok:{u'extentFreeList': {u'totalSize': 0, u'num': 0}, u'storageSize': 1205293056.0, u'ok': 1.0, u'avgObjSize': 1238.475721597792, u'dataFileVersion': {u'major': 4, u'minor': 5}, u'db': u'ceilometer', u'indexes': 15, u'objects': 824663, u'collections': 9, u'fileSize': 2080374784.0, u'numExtents': 47, u'dataSize': 1021325104, u'indexSize': 192381280, u'nsSizeMB': 16}
```

## Rabbitmq Restart检测

该命令可以用来检测 Rabbitmq 服务是否重启过。Rabbitmq 服务非正常重启可能会导致 OpenStack 组件连接 Rabbitmq 出现异常，因此需要检查 Rabbitmq 是否重启过。

```
(controller)# eayunstack --debug doctor env -n rabbitmqrestart
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_rabbitmqrestart 
[ DEBUG ] (controller) (node-15.eayun.com): service rabbitmq has never been restart
```

## SELinux检测

该命令用于检测当前节点SELinux模式及配置文件中所配置的模式，当执行该命令输出类似如下信息时当前节点SELinux模式配置正常。

```
# eayunstack --debug doctor env -n selinux
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_selinux 
[ DEBUG ] (controller) (node-5.eayun.com): SELinux current state is: Disabled
[ DEBUG ] (controller) (node-5.eayun.com): SELinux current conf in profile is: disabled
```

## Daemon检测

该命令用于检测当前节点内存使用率排序中的前三个进程的内存使用率。

> 当某个进程内存使用率超过 30% 时输出 ```WARN``` 级别消息，超过 50% 时输出 ```ERROR``` 级别消息。

```
# eayunstack --debug doctor env -n daemon
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_daemon 
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 33138(pid) usage 6.4%
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 9941(pid) usage 5.9%
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 33245(pid) usage 3.2%
```

## Disk检测

该命令用于检测当前节点“/”文件系统的空间使用率。当“/”文件系统空间使用率大于85%时，执行该命令会发出警告。

```
# eayunstack --debug doctor env -n disk
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_disk 
[ DEBUG ] (controller) (node-5.eayun.com): The "/" filesystem used 51% space !
```

## Memory检测

```
# eayunstack --debug doctor env -n memory
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_memory 
[ DEBUG ] (controller) (node-15.eayun.com): The system memory has been used 26.99%!
```

## Network检测

该命令用于检测当前节点的网卡状态。执行该命令后可查看该节点对应网卡的连接状态。

```
# eayunstack --debug doctor env -n network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_network 
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno1(br-mgmt) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno2(br-ex) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f0(br-storage) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f1(br-prv) connected
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.3(node-14) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.2(node-14) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.4(node-15) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.3(node-15) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.3(node-15) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.5(node-16) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.4(node-16) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.6(node-17) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.5(node-17) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.4(node-17) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.7(node-18) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.6(node-18) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.5(node-18) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.9(node-20) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.8(node-20) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.10(node-21) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.9(node-21) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-22.eayun.com(primary-mongo):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.11(node-22) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.8(node-24) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.7(node-24) reached --- storage network
```

## CPU检测

该命令用于检测当前节点的CPU频率是否正常。

```
# eayunstack --debug doctor env -n cpu
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_cpu 
[ DEBUG ] (controller) (node-5.eayun.com): Current CPU Frequency: 2.41 GHz
```

## CPU load检测

```
# eayunstack --debug doctor env -n cpuload
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_cpuload 
[ DEBUG ] (controller) (node-15.eayun.com): Current CPU load averages is : 3.23, 3.56, 3.20.
```

## 检测所有检测对象

该命令用于同时检测env子命令的所有检测对象。

```
# eayunstack --debug doctor env -a
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_ntp 
[ DEBUG ] (controller) (node-15.eayun.com): Service ntpd is running ...
[ DEBUG ] (controller) (node-15.eayun.com): Service ntpd is enabled ...
[ DEBUG ] (controller) (node-15.eayun.com): ntpserver is 110.75.186.249
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_mongodb 
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_rabbitmqrestart 
[ DEBUG ] (controller) (node-15.eayun.com): service rabbitmq has never been restart
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_selinux 
[ DEBUG ] (controller) (node-15.eayun.com): SELinux current state is: Disabled
[ DEBUG ] (controller) (node-15.eayun.com): SELinux current conf in profile is: disabled
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_daemon 
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 33138(pid) usage 6.4%
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 9941(pid) usage 5.9%
[ DEBUG ] (controller) (node-15.eayun.com): the memory of 33245(pid) usage 3.2%
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_disk 
[ DEBUG ] (controller) (node-15.eayun.com): The "/" filesystem used 50% space !
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_memory 
[ DEBUG ] (controller) (node-15.eayun.com): The system memory has been used 41.32%!
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_network 
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno1(br-mgmt) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno2(br-ex) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f0(br-storage) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f1(br-prv) connected
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.3(node-14) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.2(node-14) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.4(node-15) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.3(node-15) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.3(node-15) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.5(node-16) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.4(node-16) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.6(node-17) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.5(node-17) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.4(node-17) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.7(node-18) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.6(node-18) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): ping 25.0.0.5(node-18) reached --- public network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.9(node-20) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.8(node-20) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.10(node-21) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.9(node-21) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-22.eayun.com(primary-mongo):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.11(node-22) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.101.8(node-24) reached --- internal network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): ping 172.16.102.7(node-24) reached --- storage network
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_cpu 
[ DEBUG ] (controller) (node-15.eayun.com): Current CPU Frequency: 2.41 GHz
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_cpuload 
[ DEBUG ] (controller) (node-15.eayun.com): Current CPU load averages is : 4.97, 3.87, 3.71.
```