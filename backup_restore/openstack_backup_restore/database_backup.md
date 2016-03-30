# 数据库备份与恢复

## 数据库备份

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

(controller)# crm resource restart p_haproxy

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

### 将备份节点的 Pacemaker 设置为维护模式

```

(node-7)# crm node maintenance

```

### 停止MySQL同步机制

```

(node-7)# mysql -e "SET GLOBAL wsrep_on=off;"
```

### 备份数据库

```

(node-7)# xtrabackup --backup --stream=tar ./ | gzip - > backup.tar.gz

```

### 将备份文件 **backup.tar.gz** 拷贝到备份服务器

### 恢复MySQL同步机制

```

(node-7)# mysql -e "SET GLOBAL wsrep_on=on;"

```

### 将MySQL服务设置为启用模式

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# echo "enable server mysqld/node-7" | socat stdio /var/lib/haproxy/stats

```

### 将备份节点的 Pacemaker 设置为就绪模式

```

(node-7)# crm node ready

```

***

## 数据库恢复

### 停止所有 Controller 节点上的所有 OpenStack 服务

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# openstack-service stop

```

### 停止 Pacemaker 集群

> 该操作可以在 **任意 Controller 节点** 执行

```

(controller)# pcs cluster stop

```

### 备份 grastate.dat 文件

> 该操作需要在 **备份节点** (生成备份数据的节点)执行

```

(node-7)# mv /var/lib/mysql/grastate.dat /var/lib/mysql/grastate.old

```

### 将数据库备份文件 backup.tar.gz 拷贝到备份节点

### 解压备份文件并修改文件归属

> 该命令需要在 **备份节点** 执行

```

(node-7)# tar -xvzf backup.tar.gz -C /var/lib/mysql/

(node-7)# chown -R mysql.mysql /var/lib/mysql

```

### 在所有 Controller 节点导出 mysql-wss 的环境变量

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# export OCF_RESOURCE_INSTANCE=p_mysql

(controller)# export OCF_ROOT=/usr/lib/ocf

(controller)# export OCF_RESKEY_socket=/var/run/mysqld/mysqld.sock

```

### 在备份节点导出 mysql-wss 的环境变量

```

(node-7)# export OCF_RESKEY_additional_parameters="--wsrep-new-cluster"

```

### 在备份节点启动 mysqld 服务

```

(node-7)# /usr/lib/ocf/resource.d/mirantis/mysql-wss start

```

### 在剩余 Controller 节点上启动 mysqld 服务

> 该操作需要在 **剩余所有 Controller 节点** 执行

```

(controller)# /usr/lib/ocf/resource.d/mirantis/mysql-wss start

```

### 启动 Pacemaker 集群

> 该操作可以在 **任意 Controller 节点** 执行

```

(controller)# pcs cluster start

```

### 启动所有 Controller 节点上的所有 OpenStack 服务

> 该操作需要在 **所有 Controller 节点** 执行

```

(controller)# openstack-service start

```
### 确认环境中所有服务及集群状态，服务及集群状态恢复正常后确认数据库恢复是否成功