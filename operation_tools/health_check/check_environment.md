# 基础环境检测

该命令用于对EayunStack环境中的各节点进行基础环境检测，包括对NTP、Mongodb、SELinux、Disk、Memory、Network、CPU、CPU负载的检测。

## 命令格式

```
# eayunstack doctor env --help
usage: eayunstack doctor env [-h]
                             [-n {ntp,mongodb,selinux,disk,memory,network,cpu,cpuload}]
                             [-a]

Check Environment Object

optional arguments:
  -h, --help            show this help message and exit
  -n {ntp,mongodb,selinux,disk,memory,network,cpu,cpuload}
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

## SELinux检测

该命令用于检测当前节点SELinux模式及配置文件中所配置的模式，当执行该命令输出类似如下信息时当前节点SELinux模式配置正常。

```
# eayunstack --debug doctor env -n selinux
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_selinux 
[ DEBUG ] (controller) (node-5.eayun.com): SELinux current state is: Disabled
[ DEBUG ] (controller) (node-5.eayun.com): SELinux current conf in profile is: disabled
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
[ DEBUG ] (controller) (node-5.eayun.com): =====> start running check_network 
[ DEBUG ] (controller) (node-5.eayun.com): Network card eno1(br-mgmt) connected
[ DEBUG ] (controller) (node-5.eayun.com): Network card eno2(br-ex) connected
[ DEBUG ] (controller) (node-5.eayun.com): Network card enp5s0f0(br-storage) connected
[ DEBUG ] (controller) (node-5.eayun.com): Network card enp5s0f1(br-prv) connected
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-1.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.3 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-1.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.2 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-2.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.4 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-2.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.3 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-3.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.5 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-3.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.4 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-4.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.6 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-4.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.5 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-5.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.7 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-5.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.6 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping public_address of node-5.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 25.0.0.3 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-6.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.8 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-6.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.7 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping public_address of node-6.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 25.0.0.4 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-7.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.9 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-7.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.8 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-8.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.10 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-8.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.9 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping public_address of node-8.eayun.com(controller):
[ DEBUG ] (controller) (node-5.eayun.com): 25.0.0.5 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-9.eayun.com(primary-mongo):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.11 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping internal_address of node-10.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.101.12 reached
[ DEBUG ] (controller) (node-5.eayun.com): =====> start ping storage_address of node-10.eayun.com(compute):
[ DEBUG ] (controller) (node-5.eayun.com): 172.16.102.11 reached
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
[ DEBUG ] (controller) (node-15.eayun.com): ntpserver is 202.112.29.82
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_mongodb 
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_selinux 
[ DEBUG ] (controller) (node-15.eayun.com): SELinux current state is: Disabled
[ DEBUG ] (controller) (node-15.eayun.com): SELinux current conf in profile is: disabled
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_disk 
[ DEBUG ] (controller) (node-15.eayun.com): The "/" filesystem used 46% space !
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_memory 
[ DEBUG ] (controller) (node-15.eayun.com): The system memory has been used 27.02%!
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_network 
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno1(br-mgmt) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card eno2(br-ex) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f0(br-storage) connected
[ DEBUG ] (controller) (node-15.eayun.com): Network card enp5s0f1(br-prv) connected
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.3 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-14.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.2 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.4 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.3 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-15.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 25.0.0.3 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.5 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-16.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.4 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.6 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.5 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-17.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 25.0.0.4 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.7 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.6 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping public_address of node-18.eayun.com(controller):
[ DEBUG ] (controller) (node-15.eayun.com): 25.0.0.5 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.9 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-20.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.8 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.10 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-21.eayun.com(ceph-osd):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.9 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-22.eayun.com(primary-mongo):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.11 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping internal_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.101.8 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start ping storage_address of node-24.eayun.com(compute):
[ DEBUG ] (controller) (node-15.eayun.com): 172.16.102.7 reached
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_cpu 
[ DEBUG ] (controller) (node-15.eayun.com): Current CPU Frequency: 2.40 GHz
[ DEBUG ] (controller) (node-15.eayun.com): =====> start running check_cpuload 
[ DEBUG ] (controller) (node-15.eayun.com): Current CPU load averages is : 3.56, 3.63, 3.24.
```