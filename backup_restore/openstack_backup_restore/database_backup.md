# 数据库备份与恢复

需要备份的数据库包括keystone、glance、cinder、nova、neutron、ceilometer。

> #### 重要
> 该命令需要在**Controller节点**执行

## 同时备份所有数据库

```
# mysqldump --opt --all-databases > openstack.sql
```

## 单独备份指定数据库

```
# mysqldump --opt nova > nova.sql
```

## 自动备份脚本

管理员可以通过cron服务实现周期性自动化备份，下面是个数据库备份的示例脚本，执行该脚本可以备份所有OpenStack数据库，同时删除7天前的备份。

```
#!/bin/bash
backup_dir="/var/lib/backups/mysql"
filename="${backup_dir}/mysql-`hostname`-`eval date +%Y%m%d`.sql.gz"
# Dump the entire MySQL database
/usr/bin/mysqldump --opt --all-databases | gzip > $filename
# Delete backups older than 7 days
find $backup_dir -ctime +7 -type f -delete
```

## 恢复数据库

> #### 重要
> 恢复数据库前需要停止**所有节点**上与要恢复的数据库有关的**所有服务**

### 示例：恢复nova数据库

* 停止相关服务

```
# systemctl stop openstack-nova-api.service
# systemctl stop openstack-nova-cert.service
# systemctl stop openstack-nova-conductor.service
# systemctl stop openstack-nova-consoleauth.service
# systemctl stop openstack-nova-novncproxy.service
# systemctl stop openstack-nova-objectstore.service
# systemctl stop openstack-nova-scheduler.service
```

* 恢复数据库

```
# mysql nova < nova.sql
```








