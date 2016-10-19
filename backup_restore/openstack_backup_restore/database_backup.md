# 数据库备份与恢复

## 数据库备份

### 确认 MySQL 集群状态

> 备份前需要确保 MySQL 集群里的所有节点处于 Started 状态

```
(controller)# pcs resource
...
 Clone Set: clone_p_mysql [p_mysql]
     Started: [ node-1.xxx.xxx node-2.xxx.xxx node-3.xxx.xxx node-4.xxx.xxx node-5.xxx.xxx node-6.xxx.xxx node-7.xxx.xxx]
...
```

### 修改HAProxy配置

> 该操作需要在 **所有 Controller 节点** 执行 

```

(controller)# vim /etc/haproxy/haproxy.cfg

global

  daemon

  group  haproxy

  log  /dev/log local0

  maxconn  16000

  pidfile  /var/run/haproxy.pid

-  stats  socket /var/lib/haproxy/stats

+  stats  socket /var/lib/haproxy/stats level admin

  tune.bufsize  32768

  tune.maxrewrite  1024

  user  haproxy

(controller)# /usr/lib/ocf/resource.d/mirantis/ns_haproxy reload

...

```

> **注意**

>

> 同一环境中如果没有节点的 **删除或增加**，该操作只需要执行一次即可，不需要每次备份操作都执行。

>

> 环境中 **删除或新增** 节点后需要重新执行该操作

### 获取执行备份命令的节点

> 该操作可以在 **任意 Controller 节点** 执行

```

(controller)# python get_mysql_backup_node.py

node-7

```

> 在该示例中，"node-7" 即为执行数据库备份操作的节点

### 将 MySQL 服务设置为维护模式

> 该命令需要在 **所有 Controller 节点** 执行

```

(controller)# echo "disable server mysqld/node-7" | socat stdio /var/lib/haproxy/stats

```
> 确认命令执行成功
>
> 输出信息中包含 ```MAINT``` 时可以确认命令已经执行成功

```
(controller)# echo 'show stat' | socat stdio /var/lib/haproxy/stats | grep 'mysqld,node-7'
mysqld,node-7,0,0,0,0,,0,0,0,,0,,0,0,0,0,MAINT,1,0,1,0,1,227,227,,1,14,3,,0,,2,0,,0,L7OK,200,15,,,,,,,0,,,,0,0,,,,,-1,,,0,0,0,0,
```

### 将备份节点的 Pacemaker 设置为维护模式

```

(node-7)# crm node maintenance

```

> 确认命令执行成功

```
(node-7)# crm node status
<nodes>
...
    <instance_attributes id="nodes-7">
      <nvpair id="nodes-7-maintenance" name="maintenance" value="on"/>
    </instance_attributes>
...
</nodes>
```

### 停止MySQL同步机制

```

(node-7)# mysql -e "SET GLOBAL wsrep_on=off;"
```

> 确认命令执行成功

```
(node-7)# mysql -e "SHOW VARIABLES LIKE 'wsrep_on';"
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_on      | OFF   |
+---------------+-------+
```

### 压缩 keystone.token 表空间
> 由于 keystone.token 表增长较快，导致表数据文件较大，可以在备份前压缩该表空间，以加快备份还原时间。

```

(node-7)# mysql -e "OPTIMIZE TABLE keystone.token;"
```


### 备份数据库

```

(node-7)# innobackupex --stream=tar ./ | gzip - > backup.tar.gz
(node-7)# md5sum backup.tar.gz > backup.tar.gz.md5

```

### 将备份文件 **backup.tar.gz** 及 **backup.tar.gz.md5** 拷贝到备份服务器

### 恢复MySQL同步机制

```

(node-7)# mysql -e "SET GLOBAL wsrep_on=on;"

```
> 确认命令执行成功

```
(node-7)# mysql -e "SHOW VARIABLES LIKE 'wsrep_on';"
+---------------+-------+
| Variable_name | Value |
+---------------+-------+
| wsrep_on      | ON    |
+---------------+-------+
```

### 将MySQL服务设置为启用模式

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# echo "enable server mysqld/node-7" | socat stdio /var/lib/haproxy/stats

```
> 确认命令执行成功
>
> 输出信息中包含 ```UP``` 时可以确认命令已经执行成功

```
(controller)# echo 'show stat' | socat stdio /var/lib/haproxy/stats | grep 'mysqld,node-7'
mysqld,node-7,0,0,0,0,,0,0,0,,0,,0,0,0,0,UP,1,0,1,0,1,373,1214,,1,14,3,,0,,2,0,,0,L7OK,200,13,,,,,,,0,,,,0,0,,,,,-1,OK,,0,0,0,0,
```

### 将备份节点的 Pacemaker 设置为就绪模式

```

(node-7)# crm node ready

```
> 确认命令执行成功

```
(node-7)# crm node status
<nodes>
...
    <instance_attributes id="nodes-7">
      <nvpair id="nodes-7-maintenance" name="maintenance" value="off"/>
    </instance_attributes>
...
</nodes>
```

***

## 数据库恢复

### 停止所有 Controller 节点上的所有 OpenStack 服务

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# openstack-service stop

```

### 停止 Pacemaker 集群

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# pcs cluster stop

```

### 备份数据库目录

> 该操作需要在 **备份节点** (生成备份数据的节点)执行

```

(node-7)# mv /var/lib/mysql /var/lib/mysql.old
(node-7)# mkdir /var/lib/mysql

```

### 将数据库备份文件 **backup.tar.gz** 及 **backup.tar.gz.md5** 拷贝到备份节点

### 检查数据库备份文件的完整性

```
(node-7)# md5sum backup.tar.gz
4fbac699ce1cbbf33242621ea7e94afa   backup.tar.gz
(node-7)# cat backup.tar.gz.md5
4fbac699ce1cbbf33242621ea7e94afa   backup.tar.gz
```

> 两条命令输出结果相同，确认数据备份文件是完整的。

### 恢复备份数据并修改文件归属

> 该命令需要在 **备份节点** 执行

```

(node-7)# mkdir /root/backup_dir

(node-7)# tar -xivzf backup.tar.gz -C /root/backup_dir

(node-7)# innobackupex --apply-log /root/backup_dir

(node-7)# innobackupex --copy-back /root/backup_dir

(node-7)# chown -R mysql.mysql /var/lib/mysql

```

### 在备份节点导出 mysql-wss 的环境变量

```
(node-7)# export OCF_RESOURCE_INSTANCE=p_mysql

(node-7)# export OCF_ROOT=/usr/lib/ocf

(node-7)# export OCF_RESKEY_socket=/var/lib/mysql/mysql.sock

(node-7)# export OCF_RESKEY_additional_parameters="--wsrep-new-cluster"

```

### 在备份节点启动 mysqld 服务

```

(node-7)# /usr/lib/ocf/resource.d/mirantis/mysql-wss start

```

> 该命令需要在 **备份节点** 执行

### 在剩余 Controller 节点上启动 mysqld 服务

> 该操作需要在 **剩余所有 Controller 节点** 执行

```
(controller)# export OCF_RESOURCE_INSTANCE=p_mysql

(controller)# export OCF_ROOT=/usr/lib/ocf

(controller)# export OCF_RESKEY_socket=/var/lib/mysql/mysql.sock

(controller)# /usr/lib/ocf/resource.d/mirantis/mysql-wss start

```

### 启动 Pacemaker 集群

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# pcs cluster start

```

### 确保 rabbitmq 集群正常
> 由于同时启动 Pacemaker 集群，rabbitmq 可能会出现脑裂现象
> 需要进行干预，确保 rabbitmq 集群正常。


> 下面示例中的命令在任一台 Controller 节点执行即可
> 逐个禁止 rabbitmq 在除本节点之外的节点启动，然后逐个恢复
> 最终确认所有节点的 rabbitmq 实例属于同一个集群

```
(controller)# pcs resource ban p_rabbitmq-server node-xxx.xxx.xxx

...

(controller)# pcs resource ban p_rabbitmq-server node-xxx.xxx.xxx

(controller)# pcs resource
p_rabbitmq-server 只有当前节点运行

(controller)# pcs resource clear p_rabbitmq-server node-xxx.xxx.xxx
恢复 p_rabbitmq-server 在其它节点运行

(controller)# rabbitmqctl cluster_status
所有节点 rabbitmq 实例属于同一个集群
```

### 启动所有 Controller 节点上的所有 OpenStack 服务

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# openstack-service start

```
### 确认环境中所有服务及集群状态，服务及集群状态恢复正常后确认数据库恢复是否成功

### 根据需要清理备份节点上备份数据的解压目录 （ /root/backup_dir ）以及新建的数据库备份目录（ /var/lib/mysql.old ）