# 环境升级

在EayunStack环境运行过程中，可能会出现需要对环境中的某个服务的配置或某个软件包进行升级，下面介绍下如何对环境进行升级。

> **注意**
> 该操作只能在**Fuel**节点进行

## 命令格式

```
(fuel)# eayunstack upgrade --help
usage: eayunstack upgrade [-h] COMMAND ...

Upgrade Management

optional arguments:
  -h, --help  show this help message and exit

Commands:
  COMMAND     DESCRIPTION
    go
    setup
```

## 升级方法

升级过程分成两步：环境配置和环境升级。

### 环境配置

* 配置RSYNC服务器及所有OpenStack节点的YUM源

#### 命令格式

```
(fuel)# eayunstack upgrade setup --help
usage: eayunstack upgrade setup [-h] --myip MYIP

optional arguments:
  -h, --help   show this help message and exit
  --myip MYIP  IP address of the fuel node
```

#### 命令示例

```
[root@fuel ~](fuel)# eayunstack upgrade setup --myip 10.20.0.2
```

* 创建相关工作目录

```
[root@fuel ~](fuel)# mkdir -p /var/www/nailgun/eayunstack/repo
```

* 下载升级用的puppet模块

```
[root@fuel ~](fuel)# wget http://xxx.xxx.xxx.xxx/xxx.tar.gz
[root@fuel ~](fuel)# tar -czf xxx.tar.gz
[root@fuel ~](fuel)# mv /var/www/nailgun/eayunstack/puppet{,.bak-`date +%Y%m%d%H%M%S`}
[root@fuel ~](fuel)# cp -r xxx/puppet/ /var/www/nailgun/eayunstack/
```

* 安装YUM源制作工具并制作YUM源

>  **注意**
>
>  如需升级环境中的某些软件包，需要先将高版本软件包拷贝到```/var/www/nailgun/eayunstack/repo/```目录下再执行```createrepo```命令。

```
[root@fuel ~](fuel)# yum -y install createrepo
[root@fuel ~](fuel)# createrepo /var/www/nailgun/eayunstack/repo/
```

### 环境升级

#### 命令格式

```
(fuel)# eayunstack upgrade go --help
usage: eayunstack upgrade go [-h] --myip MYIP [--check-only]

optional arguments:
  -h, --help    show this help message and exit
  --myip MYIP   IP address of the fuel node
  --check-only  If specified, the program will only check the current progress
                instead of start a new upgrade process.
```

#### 命令示例

* 升级第一个Controller节点

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2
```

* 查看升级进度

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 15 is still running.
```

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 15 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
```

>  通过以上信息可以确定第一个节点升级已经成功完成。继续升级剩余节点。

* 升级剩余节点

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2
```

* 查看升级进度

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 12 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 14 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 13 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 10 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 11 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 8 is still running.
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 7 is still running.
```

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 12 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 10 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 13 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 14 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 11 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
[ INFO  ] (fuel) (fuel.domain.tld): Process on node 8 is finished.
[ INFO  ] (fuel) (fuel.domain.tld): Events:
    total: 1
    success: 1
    failure: 0
[ INFO  ] (fuel) (fuel.domain.tld): Resources:
    total: 10
    changed: 1
    failed: 0
    restarted: 1
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
    total: 10
    changed: 1
    failed: 0
    restarted: 1
    failed_to_restart: 0
    scheduled: 0
    skipped: 0
    out_of_sync: 1
```

```
[root@fuel ~](fuel)# eayunstack upgrade go --myip 10.20.0.2 --check-only
[ INFO  ] (fuel) (fuel.domain.tld): No upgrade process is currently on the way.
```

>  从以上信息中可以确认剩余节点已经成功升级完成。

## 关于升级失败

升级失败时可以根据相关提示信息到对应的失败节点查看```/var/log/puppet.log```日志文件，确定失败原因。确定失败原因并解决问题后，可以在**Fuel节点**删除```/var/run/eayunstack/```目录下的所有文件并执行升级命令尝试重新升级。