### 监控添加mongo集群配置

  监控系统采用ceilometer部署，监控存储数据使用mongodb存储，通过添加mongodb集群为监控系统配置存储集群服务，
  这里的集群环境是指mongodb复制集环境．本文主要描述将监控环境下mongo单节点环境变成复制集环境．
  在调整前，请先关闭所有ceilometer服务，调整完成后启动ceilometer服务

#### 监控系统配置文件配置

 配置集群前关闭所有ceilometer监控服务

1） 修改ceilometer配置文件，增加数据库连接节点配置
* vim /etc/ceilometer/ceilometer.conf （每个安装ceilometer集群下的ceilometer 配置文件都要配置）
* 在[database]标签下，修改connection如下：
 ` connection=mongodb://ceilometer:password@172.16.101.11,172.16.101.12,172.16.101.9/ceilometer `

 配置参数说明：

     ```
     connection=mongodb:// 采用mongodb连接
     ceilometer 为mongodb数据库用户名
     password 为mongodb数据库密码
     172.16.101.11/12/9 为mongodb数据库三个节点IP地址
     /ceilometer 为连接的数据库名称
     ```

2）修改每个节点下的astute.yaml文件，配置mongodb角色与节点的fqdn参数

```
- swift_zone: '9'
  uid: '9'
  internal_netmask: 255.255.255.0
  fqdn: node-9.eayun.com
  role: primary-mongo
  internal_address: 172.16.101.11
  storage_address: 172.16.102.10
  storage_netmask: 255.255.255.0
  name: node-9
- swift_zone: '11'
  uid: '11'
  internal_netmask: 255.255.255.0
  fqdn: node-10.eayun.com
  role: mongo
  internal_address: 172.16.101.12
  storage_address: 172.16.102.11
  storage_netmask: 255.255.255.0
  name: node-10
- swift_zone: '12'
  uid: '12'
  internal_netmask: 255.255.255.0
  fqdn: node-7.eayun.com
  role: mongo
  internal_address: 172.16.101.9
  storage_address: 172.16.102.8
  storage_netmask: 255.255.255.0
  name: node-7

```

3）配置数据库集群环境

* 节点安装mongodb软件

  ` yum install mongodb-server mongodb`

* 节点配置mongodb.conf配置文件 （文件位于/etc目录下，每个节点的ip地址不一样需要作出修改）

```
# mongo.conf - generated from Puppet

#where to log
syslog = true
logappend=true
# Set this option to configure the mongod or mongos process to bind to and
# listen for connections from applications on this address.
# You may concatenate a list of comma separated values to bind mongod to multiple IP addresses.
bind_ip = 127.0.0.1,172.16.101.12
# fork and run in background
fork=true
port = 27017
dbpath=/var/lib/mongo
# location of pidfile
pidfilepath = /var/run/mongodb/mongod.pid
# Enables journaling
journal = true
# Turn on/off security.  Off is currently the default
auth=true
# Configure ReplicaSet membership
replSet = ceilometer
# Specify the path to a key file to store authentication information.
keyFile = /etc/mongodb.key
setParameter = logLevel=1
```

配置修改完成后，需要添加mongodb.key文件，如果没有admin的密码，需要先将keyFile=与auth=True注释掉，mongodb admin
密码通过与ceilometer数据库连接密码一样，通过查看ceilometer.conf中的connection数据库连接项获取．


> mongodb.key创建 

```
openssl rand -base64 741 > /etc/mongodb.key
chmod 600 mongodb-keyfile
```

* 启动mongodb服务

```
[root@node-9 ~](mongo)# systemctl start mongod
```

登录数据库使用admin账户，在以前运行的节点登录(mongo 172.16.101.12:27017/admin -u admin -pxxxxx)
* 配置mongo集群

> 初始化复制集环境（初始化的节点，选择以前运行的节点进行初始化

```
use admin;(切换至admin数据库)
rs.initiate();(初始化环境，初始化后，查看ceilometer数据库中，确认数据库中数据存在)
rs.add('172.16.101.11:27017'); (添加节点至环境)
rs.add('172.16.101.9:27017');(添加另一个节点至环境,添加节点过程中因数据同步，因此
需要一段时间进行数据同步)
rs.status();(查看环境状态，会发现一个primary主节点，一个second节点）)

ceilometer:PRIMARY> rs.status()
{
	"set" : "ceilometer",
	"date" : ISODate("2015-09-15T06:53:18Z"),
	"myState" : 1,
	"members" : [
		{
			"_id" : 1,
			"name" : "172.16.101.9:27017",
			"health" : 1,
			"state" : 1,
			"stateStr" : "PRIMARY",
			"uptime" : 79550,
			"optime" : Timestamp(1442288970, 1),
			"optimeDate" : ISODate("2015-09-15T03:49:30Z"),
			"electionTime" : Timestamp(1442220462, 1),
			"electionDate" : ISODate("2015-09-14T08:47:42Z"),
			"self" : true
		},
		{
			"_id" : 2,
			"name" : "172.16.101.11:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 78397,
			"optime" : Timestamp(1442288970, 1),
			"optimeDate" : ISODate("2015-09-15T03:49:30Z"),
			"lastHeartbeat" : ISODate("2015-09-15T06:53:16Z"),
			"lastHeartbeatRecv" : ISODate("2015-09-15T06:53:16Z"),
			"pingMs" : 0,
			"syncingTo" : "172.16.101.9:27017"
		},
		{
			"_id" : 3,
			"name" : "172.16.101.12:27017",
			"health" : 1,
			"state" : 2,
			"stateStr" : "SECONDARY",
			"uptime" : 78397,
			"optime" : Timestamp(1442288970, 1),
			"optimeDate" : ISODate("2015-09-15T03:49:30Z"),
			"lastHeartbeat" : ISODate("2015-09-15T06:53:16Z"),
			"lastHeartbeatRecv" : ISODate("2015-09-15T06:53:16Z"),
			"pingMs" : 0,
			"syncingTo" : "172.16.101.9:27017"
		}
	],
	"ok" : 1
}
ceilometer:PRIMARY>

在数据库中，认证一下ceilometer 密码
use ceilometer; (切换至ceilometer数据库)
rs.auth('ceilometer','xxxx');(认证一下数据库密码，新增复制集节点后)
```


> 使用ceilometer用户连接数据库

` [root@node-9 ~](mongo)#mongo 172.16.101.9:27017/ceilometer -u ceilometer -pxxxxx`
正常登录后，确定mongo复制集配置正确

> 退出登录环境

`ceilometer:PRIMARY>quit()`

4) 重启ceilometer服务
　在重启ceilometer服务前，检查所有ceilometer服务所在controller节点能够正常访问新增mongo节点的mongo数据库端口
　重启ceilometer 所有服务

```
controller节点启动ceilometer服务:

systemctl start openstack-ceilometer-api.service openstack-ceilometer-alarm-notifier.service openstack-ceilometer-collector.service 
openstack-ceilometer-notification.service 
pcs resource enable p_openstack-ceilometer-central
pcs resource enable p_openstack-ceilometer-alarm-evaluator

compute节点启动ceilometer服务:
systemctl start openstack-ceilometer-compute.service

```

5) 验证ceilometer环境服务正常
查看ceilometer服务状态
查看ceilometer服务日志
通过api查询数据库中数据

```
[root@node-9 ~](controller)# ceilometer sample-list -m cpu_util -l 2
+--------------------------------------+----------+-------+--------+------+---------------------+
| Resource ID                          | Name     | Type  | Volume | Unit | Timestamp           |
+--------------------------------------+----------+-------+--------+------+---------------------+
| 7b2196fe-4185-4e3e-bd73-07d1f56a66e7 | cpu_util | gauge | 0.0    | %    | 2016-02-15T02:20:13 |
| 7b2196fe-4185-4e3e-bd73-07d1f56a66e7 | cpu_util | gauge | 0.0    | %    | 2016-02-15T02:19:13 |
+--------------------------------------+----------+-------+--------+------+---------------------+

```

服务一切正常，ceilometer监控环境将mongo单节点调整为复制集环境完成．

备注：在此过程中需要注意，mongo配置文件中的ＩＰ地址需要换成正式的ＩＰ，初始化与添加节点时需要用admin登录
