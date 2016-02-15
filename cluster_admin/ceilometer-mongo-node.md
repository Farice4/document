#### mongodb集群节点管理

  Mongodb 集群管理，主要介绍mongodb在现有集群中新增与删除集群。

##### Mongodb集群新增节点

* 节点安装mongodb-server,mongodb服务

`yum install mongodb-server mongodb`

* 配置节点mongodb.conf配置文件，根据现有节点配置文件修改，其中需要对bind_ip地址作出修改
* 将mongodb.key文件拷贝过来放到配置文件中指定位置
* 启动节点mongod服务

`[root@node-9 ~](mongo)#systemctl start mongod`

* 使用管理员用户登录mongodb数据库，进行节点添加

> 登录数据库主库

`[root@node-9 ~](mongo)#mongo 172.16.101.9:27017/admin -u admin -p` 根据提示输入sa密码进入数据库

> 添加新的节点

`ceilometer:PRIMARY>rs.add("172.16.101.10:27017");` (添加节点）

等待一段时间后，节点同步数据完成，节点状态正常

> 查看节点信息

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
		}，
		{
			"_id" : 4,
			"name" : "172.16.101.10:27017",
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

* 节点登录测试

```
[root@node-9 ~](mongo)#mongo 172.16.101.10:27017/admin -u admin -p
MongoDB shell version: 2.6.11
Enter password:
connecting to: 172.16.101.10:27017/admin
ceilometer:SECONDARY>
```

节点添加完成

##### Mongodb集群删除节点

* 停止需要删除的Mongodb节点mongod服务

`[root@node-9 ~](mongo)#systemctl stop mongod`

* 使用mongodb 管理员用户登录mongodb数据库主库，删除节点

> 查看节点状态

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
		}，
		{
			"_id" : 4,
			"name" : "172.16.101.10:27017",
			"health" : 0,
			"state" : 8,
			"stateStr" : "not reachable/healthy",
			"uptime" : 0,
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
通过以上状态可以看出id:4 172.16.101.10:27017该节点已经关机

> 删除节点

`ceilometer:PRIMARY>rs.remove("172.16.101.10:27017");` (执行该命令后，节点删除）

通过上面的查看节点状态操作，可以看到节点已删除只存在3个节点

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

* 通过系统命令删除在数据库中已删除节点的数据文件及其它文件配置文件

节点删除完成

备注：在为ceilometer添加mongo复制集时需要调整ceilometer.conf中connection数据库连接项与每个节点下的astute.yaml文件，与服务重启，服务重启操作可以在数据库添加与删除完成后，重启ceilometer服务
