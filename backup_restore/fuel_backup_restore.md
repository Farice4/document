# Fuel备份与恢复

Fuel节点在EayunStack环境中担任了部署服务器的角色，当我们需要对EayunStack环境进行部署及扩容操作时，都需要依赖于Fuel节点，因此，保证Fuel节点的可用性是一项重要任务。为了防止Fuel节点由于某些原因导致数据丢失造成Fuel节点不可用，管理员需要定期对Fuel节点进行备份。

## 备份内容

* 所有的 docker container，包括 Fuel 数据库
* PXE 的部署配置
* OpenStack 的所有环境配置
* 包的仓库
* 部署的 SSH key
* Puppet manifest

## 备份Fuel节点

> ###### 注意
> 所有备份文件存放在“/var/backup/fuel/”目录下，建议将该目录备份到其它存储介质中，防止由于Fuel节点磁盘损坏导致备份文件丢失。

> ## 警告
> 备份时要满足以下两个条件：<br />
> * 没有正在进行的部署任务
> * "/var/backup/fuel/“目录下至少有 11 GB 的可用空间

```
[fuel]$ eayunstack fuel backup -n
[ INFO  ] Starting backup ...
[ INFO  ] It will take about 30 minutes, Please wait ...
[ INFO  ] Backup successfully completed!

You can use "eayunstack fuel backup [ -l | --list ]" to list your backups
```

## 恢复Fuel节点

* 列出可用于恢复的备份

```
[fuel]$ eayunstack fuel backup -l
+----+------------------+-------------------------------------+
| ID |   Backup Time    |             Backup File             |
+----+------------------+-------------------------------------+
| 1  | 2015-04-19 12:05 | fuel_backup_2015-04-19_1205.tar.lrz |
| 2  | 2015-04-19 12:09 | fuel_backup_2015-04-19_1209.tar.lrz |
| 3  | 2015-04-19 12:12 | fuel_backup_2015-04-19_1212.tar.lrz |
+----+------------------+-------------------------------------+
```

* 恢复某个备份

```
[fuel]$ eayunstack fuel restore -i 3
[ INFO  ] Starting Restore ...
[ INFO  ] It will take about 30 minutes, Please wait ...

[ INFO  ] Restore successfully completed!
```
