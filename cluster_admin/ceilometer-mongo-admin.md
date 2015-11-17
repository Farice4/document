### 监控添加mongo集群配置

  监控系统采用ceilometer部署，监控存储数据使用mongodb存储，通过添加mongodb集群为监控系统配置存储集群服务

#### 监控系统配置文件配置

 配置集群前关闭所有ceilometer监控服务

1） 修改ceilometer配置文件，增加数据库连接节点配置
* vim /etc/ceilometer/ceilometer.conf （每个安装ceilometer集群下的ceilometer 配置文件都要配置）
* 在[database]标签下，修改connection= 如下：
 ` connection=mongodb://ceilometer:password@172.16.101.11,172.16.101.12,172.16.101.9/ceilometer `

 配置参数说明：
     ```
     connection=mongodb:// 采用mongodb连接
     ceilometer 为mongodb数据库用户名
     password 为mongodb数据库密码
     172.16.101.11/12/9 为mongodb数据库三个节点IP地址
     /ceilometer 为连接的数据库名称
     ```

2）修改每个mongodb节点下的astute.yaml文件，配置mongodb角色与节点的fqdn参数

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
3） 通过mongodb数据库密码连接至mongodb admin数据库

4）配置数据库集群环境

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
bind_ip = 127.0.0.1,172.16.101.11
# fork and run in background
fork=true
port = 27017
dbpath=/var/lib/mongo/mongo
# location of pidfile
pidfilepath = /var/run/mongodb/mongod.pid
# Enables journaling
# Turn on/off security.  Off is currently the default
auth=true
setParameter = logLevel=2
smallfiles = true
journal = true
#smallfiles = true
replSet=ceilometer
keyFile = /etc/mongodb.key

```
`auth=true与keyFile=可以等集群配置完成后，创建了管理员用户后，启用认证服务后配置，先注释掉这两项配置`
* 启动mongodb服务

`[root@node-9 ~](mongo)# systemctl start mongod`

* 配置mongo集群

> 添加配置config,包括每个节点IP地址与端口
```
> config={_id : 'dbset',members : [{_id : 1, host : '172.16.101.9:27017'},{_id : 2, host : '172.16.101.11:27017'},{_id : 3, host : '172.16.101.12:27017'}]}
```
> 数据库初始化配置

```
> s.initiate(config);
```
等待一段时间后，配置

> 通过rs.status查看集群状态

```
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
```

* 为mongodb添加管理用户sa

> 连接数据库

` [root@node-9 ~](mongo)#mongo 172.16.101.9:27017/admin`

> 添加sa用户

`ceilometer:PRIMARY>use admin;` (切换到admin数据库）

`ceilometer:PRIMARY>db.addUser('sa','password')`  （password为密码，根据情况将密码设置成复杂密码）

> 认证用户，是用户生效

`ceilometer:PRIMARY>db.auth('sa','password')`

> 退出登录环境

`ceilometer:PRIMARY>quit()`

5）打开数据库登录认证，添加ceilometer用户

* 关闭每个mongo节点的mongod服务(systemctl stop mongod)

* 修改mongodb.conf配置文件中的认证注释取消掉

```
  vim /etc/mongodb.conf
  #auth=true
  #keyFi=/etc/mongo.key
  修改为
  auth=true
  keyFi=/etc/mongo.key
```
* 使用sa用户登录进mongodb数据库主节点

`[root@node-9 ~](mongo)#mongo 172.16.101.9:27017/admin -u sa -p` 根据提示输入sa密码进入数据库

* 创建ceilometer 数据库

`ceilometer:PRIMARY>use ceilometer;` （创建ceilometer数据库）

* 创建ceilometer 用户

切换到ceilometer库后，添加cerilometer用户

`ceilometer:PRIMARY>db.addUser('ceilometer','password');`

认证用户使添加用户生效

`ceilometer:PRIMARY>db.auth('ceilometer','password');`

* 退出登录，使用ceilometer用户登录，确认ceilometer能够正常登录到ceilometer数据库，该用户只对ceilometer数据库具有dbowner权限

6）启动ceilometer 所有服务，监控系统添加从数据库服务完成


