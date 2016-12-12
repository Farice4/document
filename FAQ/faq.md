* [ horzion上vnc加载失败 ](#horzion上vnc加载失败)
* [ 重启libvirt ](#重启libvirt)
* [ 备份volume ](#备份volume)
* [ 恢复volume ](#恢复volume)
* [ 恢复rabbitmq集群 ](#恢复rabbitmq集群)
* [ 配置 nova vnc console 支持 https](#配置 nova vnc console 支持 https)
* [ windows 虚拟机支持多核 CPU](#windows虚拟机支持多核CPU)
* [ openstack 数据库备份故障恢复 ](#openstack 数据库备份故障恢复)

## horzion上vnc加载失败
* 用 eayunstack 运维工具检查 EayunStack 集群是否正常
* 尝试刷新重新加载 url
* 登录控制节点, 使用 ```nova get-vnc-console [ID]```, 得到 novnc 的url, 检查改url是否和浏览器上的 novnc 地址一致.
* 检查云主机是否正常运行. 使用 nova show [ID] 得到云主机运行的计算节点, 登录计算节点, 使用 ```ps aux | grep [ID]``` 查看云主机对应的 qemu-kvm 进程是否在运行.
* 在各个 controller 节点重启 nova-consoleauth 和 nova-novncproxy 服务

***

## 重启libvirt
如果由于某些原因(比如重新配置log级别定位问题, 或者libvirt发生错误)需要重启 libvirt, 由于计算服务依赖于libvirt服务, 并且不会自动重连libvirt, 所以相应的 nova-compute 也需要重启, 步骤如下(假设重启的节点为 node-1):
* 在 node-1 使用以下指令重启libvirt
```
# systemctl libvirtd.service restart
```
* 在 node-2 使用以下指令重启 nova-compute
```
# systemctl openstack-nova-compute.service restart
```

> ###### 注意
> * libvirt 和 nova-compute 服务只运行在计算节点
> * 在哪一台计算节点上重启了 libvirt 服务, 那么必须在这台计算节点重启 nova-compute 服务

***

## 备份volume
* 确保 cinder-backup 服务处在运行状态, 如果该服务没有运行, 启动它
```
# systemctl start openstack-cinder-backup
```
* 然后使用以下指令备份 volume
```
# cinder backup-create --display-name [name] [volumeID]
```
其中 **name** 是你自己取的备份的名字, **volumeID** 是你要备份的卷的ID
* 等待 ```cinder backup-list [name]``` 的 Status 由 creating 变为 available, 表示备份完成.
* 关闭 backup 服务
```
# systemctl stop openstack-cinder-backup
```

> ###### 注意
> * cinder 备份操作只能通过命令行或者 API 实现, horizon 暂不支持
> * 由于 eqlx 存储最大管理连接数的限制, 每个 backup服务要占用一个连接session, 影响 cinder-volume 的使用, cinder backup 按需开启, 即需要备份和恢复的时候打开该服务, 完成备份关闭该服务.
> * 备份时卷状态必须在"available" 无云主机使用

***

## 恢复volume
* 确保 cinder-backup 服务处在运行状态, 如果该服务没有运行, 启动它
```
# systemctl start openstack-cinder-backup
```
* 然后使用以下指令恢复 volume
```
# cinder backup-restore [backupID]
```
其中 **backupID** 是你要恢复的卷的ID, 通过 ```cinder backup-list``` 获取
* 等待 ```cinder list``` 中恢复的卷的 Status 由 restoring-backup 变为 available, 表示恢复完成.
* 关闭 backup 服务
```
# systemctl stop openstack-cinder-backup
```

> ###### 注意
> * cinder 恢复操作只能通过命令行或者 API 实现, horizon 暂不支持
> * 恢复时只能指定要恢复之前备份的卷的ID, 通过 ```cinder backup-list``` 获取, 不能指定名字
> * 恢复时需要注意恢复使用的是cinder backup-list 查询的id恢复

***

## 恢复rabbitmq集群
* 通过 pacemaker 确认 rabbitmq 服务状态
```
pcs resource show
```
正常情况下命令会显示 p_rabbitmq-server 在各个控制节点上都是 Started 状态，示例：

    [root@node-1 ~]# pcs resource show
                ... ...
    Clone Set: clone_p_rabbitmq-server [p_rabbitmq-server]
        Started: [ node-1.eayun.com node-2.eayun.com node-3.eayun.com ]
                ... ...

如果有某一控制节点不在 Started 列表中，则相应节点上 rabbitmq 服务状态存在问题。
* 通过 rabbitmqctl 命令进一步确认 rabbitmq 集群的状态
```
rabbitmqctl cluster_status
```
正常情况下命令会显示各控制节点名称在 nodes 和 running_nodes 列表中，示例：

    [root@node-1 ~]# rabbitmqctl cluster_status
    Cluster status of node 'rabbit@node-1' ...
    [{nodes,[{disc,['rabbit@node-1','rabbit@node-2','rabbit@node-3']}]},
    {running_nodes,['rabbit@node-2','rabbit@node-3','rabbit@node-1']},
    {cluster_name,<<"rabbit@node-1.eayun.com">>},
    {partitions,[]}]
    ...done.
如果某一控制节点不在 nodes 或 running_nodes 列表中，则相应节点出现故障；
另外 partitions 一行列表正常情况为空，如果不为空也意味着集群已故障。
* 恢复故障节点
如果只是单一节点 rabbitmq 服务故障，可通过重启节点上的 rabbitmq 来使其恢复。
```
pcs resource ban p_rabbitmq-server <节点名称>
pcs resource clear p_rabbitmq-server <节点名称>
```
如果是整个 rabbitmq 集群出现故障，则需要按如下步骤恢复集群：

1. 使用如下命令依次停掉所有节点上的 p_rabbitmq-server 资源。

    ```
    pcs resource ban p_rabbitmq-server <节点名称>
    ```

2. 只启动某一节点上的 p_rabbitmq-server 资源，等启动完成再启动其他节点上的 p_rabbitmq-server 资源。

    ```
    pcs resource clear p_rabbitmq-server <第一个节点名称>
    ```

    第一个节点启动完成后，再启动其它节点

    ```
    pcs resource clear p_rabbitmq-server <其它节点名称>
    ```
3. 操作完成以后，再确认一下集群状态

    ```
    rabbitmqctl cluster_status
    ```

4. 最后，由于很多 openstack 服务不能及时的断线重连 rabbitmq，故而需要手动重启各 openstack 服务。

#### 重启以下服务对环境影响较小，可在任意时间执行

* Controller 节点

```
systemctl restart neutron-server \
openstack-nova-api openstack-nova-cert \
openstack-nova-conductor openstack-nova-consoleauth \
openstack-nova-novncproxy openstack-nova-objectstore \
openstack-nova-scheduler openstack-cinder-api \
openstack-cinder-volume openstack-cinder-scheduler \
openstack-heat-api-cfn openstack-heat-api-cloudwatch \
openstack-heat-api openstack-keystone \
openstack-glance-api openstack-glance-registry \
neutron-qos-agent openstack-ceilometer-api \
openstack-ceilometer-collector \
openstack-ceilometer-notification
```

> 注意
>
> 以下服务 ```disable``` 后，需要在所有 Controller 节点通过 ```ps``` 命令查看进程信息，确认没有服务对应的进程后，再做 ```enable``` 操作

```
pcs resource disable clone_p_neutron-metadata-agent
pcs resource enable clone_p_neutron-metadata-agent
pcs resource disable p_neutron-dhcp-agent
pcs resource enable p_neutron-dhcp-agent
pcs resource disable clone_p_openstack-heat-engine
pcs resource enable clone_p_openstack-heat-engine
pcs resource disable p_openstack-ceilometer-central
pcs resource enable p_openstack-ceilometer-central
```

* Compute 节点

```
systemctl restart openstack-nova-compute
systemctl restart neutron-qos-agent
systemctl restart openstack-ceilometer-compute
```

#### 重启以下服务对环境影响较大，需要在用户操作较少时执行

* Controller 节点

> 注意
>
> 以下服务 ```disable``` 后，需要在所有 Controller 节点通过 ```ps``` 命令查看进程信息，确认没有服务对应的进程后，再做 ```enable``` 操作

```
pcs resource disable p_neutron-l3-agent
pcs resource disable p_neutron-openvswitch-agent
pcs resource enable p_neutron-openvswitch-agent
pcs resource enable p_neutron-l3-agent
pcs resource disable p_neutron-lbaas-agent
pcs resource enable p_neutron-lbaas-agent
```

```
systemctl restart neutron-qos-agent
```

* Compute 节点

```
systemctl restart neutron-openvswitch-agent
```

> ###### 注意
>
> * 除非整个 rabbitmq 集群故障，否则不要重启所有 openstack 服务。

### PCS服务出现unmanage状态

  通常执行pcs resource 后会出现服务处于unmanage状态，需要通过如下命令将unmanage状态进行恢复

  ```
  pcs resource cleanup resource_id
  ```

  当执行以上命令后，再次执行`pcs resource`如果有需要进行ban的服务，需要按照上面的步骤进行服务重新启动

## 配置 nova vnc console 支持 https


## windows虚拟机支持多核CPU

* 上传 Windows 镜像

* 确认 Windows 镜像对应的操作系统版本支持的最大 CPU Socket 数量

* 更新 Windows 镜像的元数据，定义该镜像支持的最大 CPU Socket 数量，例如 Windows Server 2008 R2 标准版，该版本支持的最大 CPU Socket 数量为4，镜像元数据更新命令如下：
    ```
    (controller)# glance image-update --property hw_cpu_max_sockets=4 IMAGE_UUID
    ```

## openstack 数据库备份故障恢复

### 故障现象

数据库备份过程中，进行 [数据库数据文件备份](backup_restore/openstack_backup_restore/database_backup.md#备份数据库) 的步骤，可能会长时间卡在如下位置无法完成备份：

```
>> log scanned up to (xxxxxxxxxx)
```

同时环境中 keystone api 的访问也出现异常，keystone 日志中出现如下信息：

```
(OperationalError) (2013, 'Lost connection to MySQL server during query')
```

查看数据库访问时会看有Waiting for commit lock 和 FLUSH TABLES WITH READ LOCK

```
$ mysql -e 'show full processlist'
+------+-------------+-----------+------+---------+-------+-------------------------+-----------------------------+----------+
| Id   | User        | Host      | db   | Command | Time  | State                   | Info                        | Progress |
+------+-------------+-----------+------+---------+-------+-------------------------+-----------------------------+----------+
|    1 | system user |           | NULL | Sleep   | 14072 | wsrep aborter idle      | NULL                        |    0.000 |
|    2 | system user |           | NULL | Sleep   |  1027 | committed 12224277      | NULL                        |    0.000 |
|    3 | system user |           | NULL | Sleep   |  1027 | committed 12224276      | NULL                        |    0.000 |
|    4 | system user |           | NULL | Sleep   |  1027 | committed 12224275      | NULL                        |    0.000 |
|    5 | system user |           | NULL | Sleep   |  1027 | Waiting for commit lock | NULL                        |    0.000 |
| 3076 | root        | localhost | NULL | Sleep   |  1027 | NULL                    | FLUSH TABLES WITH READ LOCK |    0.000 |
| 3078 | root        | localhost | NULL | Query   |     0 | NULL                    | show full processlist       |    0.000 |
+------+-------------+-----------+------+---------+-------+-------------------------+-----------------------------+----------+
```

### 故障解决方法

#### 停止备份节点的数据库服务

* 获取 HA 集群中数据库资源的配置参数

> 手动操作 HA 数据库资源时，需要上述的 ```test_user``` 、 ```test_password``` 、```socket``` 属性。

```
# pcs resource show p_mysql
 Resource: p_mysql (class=ocf provider=mirantis type=mysql-wss)
  Attributes: test_user=wsrep_sst test_passwd=sTNX2iFv socket=/var/lib/mysql/mysql.sock
  Operations: monitor interval=120 timeout=115 (p_mysql-monitor-120)
              start interval=0 timeout=475 (p_mysql-start-0)
              stop interval=0 timeout=175 (p_mysql-stop-0)
```

* 设置环境变量

> HA 数据库资源的所有属性都需要设置为环境变量

```
# export OCF_RESKEY_test_user=wsrep_sst
# export OCF_RESKEY_test_passwd=sTNX2iFv
# export OCF_RESKEY_socket=/var/lib/mysql/mysql.sock
# export OCF_ROOT=/usr/lib/ocf
```

* 手动停止备份节点的数据库服务

> 设置好上一步中的环境变量之后，在当前 shell 环境（因为需要刚才设置的环境变量，数据库服务的 HA agent 脚本才能顺利执行）

```
# /usr/lib/ocf/resource.d/mirantis/mysql-wss stop
```

> 等待上述命令正常完成

#### 删除备份节点数据库数据目录

> 不直接进行删除，需要保留备份以便出现新的问题时进行恢复尝试。

```
# mv mysql mysql.`date +%y%m%d%H%M%S`
# mkdir mysql
# chown mysql:mysql mysql
```

#### 重新启动备份节点数据库服务

* 确认当前 shell 环境仍然有之前设置的环境变量：

```
# env | grep OCF
OCF_RESKEY_test_user=wsrep_sst
OCF_RESKEY_test_passwd=sTNX2iFv
OCF_RESKEY_socket=/var/lib/mysql/mysql.sock
OCF_ROOT=/usr/lib/ocf
```

* 执行命令

```
# /usr/lib/ocf/resource.d/mirantis/mysql-wss start
```

> 等待上述命令正常完成后验证数据库服务

```
# mysql -e 'show variables like "wsrep_on";'
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_on      | ON    |
+---------------+-------+
```

#### 继续进行其它操作

[重新进行备份](backup_restore/openstack_backup_restore/database_backup.md#停止mysql同步机制) 或跳过备份，[对 Mysql 集群进行恢复](backup_restore/openstack_backup_restore/database_backup.md#恢复mysql同步机制)

***

## 配置nova vnc https

> 获取ca.pem 文件

正式环境使用注册的认证文件, 合并为ca.pem文件

```
1、 申请的密钥文件.crt 证书文件中可能包括多个部分, 选取其中的第一部分粘贴至一个新的.crt文件
2、 查看申请的.key文件, 如果文件中存在语法错误等, 需要手动将.key文件中语法错误部分删除
3、 使用cat 新的.crt文件 .key文件 | tee ca.pem ( 如： cat ca.crt ca.key | tee ca.pem）
```

> nova 配置ssl目录

创建ssl 目录, 将生成的.pem文件拷贝至ssl 目录

将ssl 目录拷贝至controller节点的/etc/nova/ 目录下, 同时将ssl 目录拷贝到所有controller节点的/etc/nova/ 目录下

> 配置novnc https关于haproxy(每个controller节点都要执行)

*  配置/etc/haproxy/conf.d/170-nova-novncproxy.cfg文件

```
修改文件中
bind 25.0.0.2:6080
为：
bind 25.0.0.2:6080 ssl crt /etc/nova/ssl/ca.pem   (文件内容修改增加了ca.pem认证的配置）
```

* 重启haproxy服务

```
# pcs resource ban p_haproxy node-15.eayun.com  (停止node-15上的haproxy服务)
# pcs resource clear p_haproxy node-15.eayun.com  (启动node-15上的haproxy服务)

```

启动后需要查看服务能否正常启动, 如果不能正常启动需要查看node-15下的 /var/log/daemon.log日志, 如果服务不能启动, 能够查询到haproxy不能启动原因, 这里进行了证书认证配置, 因此不能启动可能与证书有关, 需要查看ca.pem证书信息

完成node-15 haproxy服务正常启动后, 依次启动其它controll节点上的haproxy服务

> compute节点配置nova.conf文件(每个compute节点都要配置)

```
修改/etc/nova/nova.conf文件中的
novncproxy_base_url=http://25.0.0.2:6080/vnc_auto.html
为：
novncproxy_base_url=https://25.0.0.2:6080/vnc_auto.html
```

* 重启compute服务(每一台compute节点都要执行)

完成nova.conf配置后需要重启compute服务, 使配置生效

```
# systemctl restart openstack-nova-compute
```

* 测试novnc https访问

登录horizon控制台, 选择云主机, 打开云主机控制台, 发现控制台加载变为https://25.0.0.2:6080/vnc_auto.html?token=d85bc482-624c-4f17-86a3-1a7acc9aec6b&title=blkart-win-utc2%284774e22a-2b34-4c3a-b113-7b13715765c8%29

确认novnc https访问成功, 配置完成
