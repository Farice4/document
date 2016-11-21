# 环境升级

> **注意**
>
> 升级操作只能在**Fuel**节点进行

> **注意**
>
> **升级前先备份一次数据库及所有节点配置文件**

## 升级步骤

升级分三步进行：升级环境准备、升级和升级验证

### 升级环境准备

> **注意**
>
> ***升级环境准备*** 操作只需在第一次环境升级前执行，环境再次升级时，以上操作不需要再次执行。

* 备份本地已存在的升级源

```
(fuel)# mv /var/www/nailgun/eayunstack{,.bak}
```

* 重新创建本地升级源目录


```
(fuel)# mkdir /var/www/nailgun/eayunstack/
```

* 安装 s3cmd 命令

> 后面步骤中需要使用该命令将公网升级源同步到本地，安装该命令时需要配置 CentOS7 及 EPEL7 YUM 源。

```
(fuel)# yum -y install s3cmd
```

> **注意：** 需要使用官方源进行安装！安装完成后，**需要删除**系统中 CentOS 及 EPEL YUM 源配置文件。

* 添加 DNS 服务器

添加 DNS 服务器 202.106.0.20

```
(fuel)# vim /etc/resolv.conf
nameserver 202.106.0.20
```

* 确认 DNS 服务器能够解析升级源的域名

> 具体域名向升级源维护者索取

* 下载（或拷贝） s3cmd 命令访问 eos 升级源时使用的配置文件".s3cfg.eayundevops"到本地"/root/"目录

> 向升级源维护者索取


* 清除已经无效的 network namespace

> 注意：
>
> 该操作需要在 **所有 Controller** 节点执行

```
(controller)# neutron-netns-cleanup
```

* 设置命令行记录命令执行时间及实时保存命令历史

将以下内容添加到 Fuel 及所有的 OpenStack 节点的 ```/etc/profile``` 文件中

```
HISTTIMEFORMAT="%F %T  "

# append to history, don't overwrite it
shopt -s histappend

# Save and reload the history after each command finishes
export PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
```

***

### 升级

* 将公网升级源同步到本地

```
(fuel)# s3cmd -c /root/.s3cfg.eayundevops --no-preserve --delete-removed sync s3://eayunstack-upgrade/ /var/www/nailgun/eayunstack/
```

> **注意：**
>
> 以下命令为命令示例，“--myip 10.20.0.2”中的 IP 地址**需要替换**为所升级环境的 Fuel 节点的 **PXE网络** IP 地址。

* 确认当前环境运行正常

> 执行以下命令确认没有任何**"WARN"**及**"ERROR"**级别的输出消息
>
> **注意：**如果环境中存在新增节点，关于新增节点网络检测相关的报错可以暂时忽略，待新增节点升级 EayunStack Tools 到最新版本后确认报错不再重现

```
(fuel)# eayunstack doctor all
```

* 配置RSYNC服务器及所有OpenStack节点的YUM源

```
(fuel)# eayunstack upgrade setup --myip 10.20.0.2
```

* 清空所有 OpenStack 节点上的 YUM 源缓存

> 注意：下面命令中的 ```n``` 需要根据具体环境中节点的数量进行替换

```
(fuel)# for i in `seq 1 n`;do ssh node-$i yum clean all;done
```

* 确认当前环境中没有正在运行的升级任务

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): No upgrade process is currently on the way.
```

* 升级第一个 Controller 节点

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2
```

* 查看第一个 Controller 节点升级进度

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 3 is still running.
```

升级任务仍在运行。

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 3 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 35
    success: 35
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 99
    changed: 35
    failed: 0
    restarted: 24
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 35
```

升级任务成功完成。

* 升级剩余所有 OpenStack 节点

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2
```


* 查看升级进度

> 通过类似以下输出确认所有节点升级成功完成。

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 6 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 16
    changed: 1
    failed: 0
    restarted: 0
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 9 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 15
    changed: 1
    failed: 0
    restarted: 0
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 5 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 11
    success: 11
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 33
    changed: 11
    failed: 0
    restarted: 4
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 11
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 13 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 3
    success: 3
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 15
    changed: 3
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 3
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 4 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 11
    success: 11
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 33
    changed: 11
    failed: 0
    restarted: 4
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 11
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 8 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 16
    changed: 1
    failed: 0
    restarted: 0
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 7 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 16
    changed: 1
    failed: 0
    restarted: 0
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 1 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 35
    success: 35
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 99
    changed: 35
    failed: 0
    restarted: 24
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 35
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 12 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 3
    success: 3
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 15
    changed: 3
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 3
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 2 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 33
    success: 33
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 99
    changed: 33
    failed: 0
    restarted: 26
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 33
```

* 确认本次升级完成

```
(fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): No upgrade process is currently on the way.
```

* 备份所有 OpenStack 节点 "/var/log/puppet.log" 文件

### 升级验证

* 确认环境运行正常

> 执行以下命令确认没有任何**"WARN"**及**"ERROR"**级别的输出消息
>
> **注意：**如果环境中存在新增节点，关于新增节点网络检测相关的报错可以暂时忽略，待新增节点升级 EayunStack Tools 到最新版本后确认报错不再重现

```
(fuel)# eayunstack doctor all
```

* 在环境中批量创建多个虚拟机，确认能够创建成功

## 关于升级失败

升级失败时可以根据相关提示信息到对应的失败节点查看```/var/log/puppet.log```日志文件，确定失败原因。确定失败原因并解决问题后，可以在**Fuel节点**删除```/var/run/eayunstack/```目录下的所有文件并执行升级命令尝试重新升级。