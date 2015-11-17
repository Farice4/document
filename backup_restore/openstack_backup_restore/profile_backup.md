# 配置文件备份与恢复

需要备份的配置文件目录如下表所示：

|组件|配置文件目录|节点|
|----|----|----|
|nova|/etc/nova|controller</br>compute|
|glance|/etc/glance|controller|
|keystone|/etc/keystone|controller|
|cinder|/etc/cinder|controller|
|neutron|/etc/neutron|controller</br>compute|
|ceilometer|/etc/ceilometer|controller</br>compute|
|rabbitmq|/etc/rabbitmq|controller|
|mysql|/etc/mysql|controller|
|ceph|/etc/ceph|controller</br>compute</br>ceph-osd|

## 备份配置文件

管理员可在各节点通过cron服务对相应配置文件进行周期性自动化备份。备份脚本示例：

```
#!/bin/bash
backup_dir="/var/lib/backups/profile"
if ! [ -e ${backup_dir} ]; then
    mkdir -p ${backup_dir}
fi
filename="${backup_dir}/nova-`hostname`-`eval date +%Y%m%d%H%M`.tgz"
tar -czf $filename /etc/nova
```

## 恢复配置文件

> #### 重要
> 恢复配置文件前需要保留当前配置文件目录

```
# mv /etc/nova{,.orig}
# tar -xzf nova-node-3.domain.tld-20150420.tgz -C /
```
