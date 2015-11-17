####MongoDB安装

> 通过RPM包安装

* 安装Mongodb服务软件mongodb-server

  [root@node-9 ~](mongo)#yum install mongodb-server

* 安装Mongodb客户端软件mongodb

  [root@node-9 ~](mongo)#yum install mongodb


> 通过二进制源码包安装

  二进制源码包安装，直接下载文件，解压文件后，通过配置即可启动mongodb服务，启动方式一般为mongod -f mongodb.conf

####MongoDB常用配置

  Mongodb配置通过配置mongodb.conf文件进行配置，常用配置如下：

  ```
  bind_ip: 配置服务绑定IP地址，配置为本地服务器IP地址
  fork: 以守护进程方式运行Mongodb
  port: 配置服务启动后绑定端口
  dbpath: 配置mongodb启动后，数据文件存储位置
  pidfilepath: 配置mongodb启动后PID文件
  auth: 配置启用登录认证
  setParameter: 配置mongodb日志存储
  smallfiles: 使用最小方式启动数据库，不配置此项，启动时通过比例计算数据存储文件占用空间大小
  journal: 写日志
  replSet: 配置副本集名称
  keyFile: 配置认证key文件
  ```

  以上在配置副本集中：replSet、keyFile必须配置

> keyFile 文件生成

  使用`openssl rand -base64 100 > /mongodb/scheme2/file.key` --文件内容采base64编码，一共100个字符,key文件的权限为`600`且属主别需为mongodb

### 常用命令

####服务启动与停止

```
[root@node-9 ~](mongo)# systemctl start mongod （服务启动）
[root@node-9 ~](mongo)# systemctl stop mongod  （服务停止）
[root@node-9 ~](mongo)# systemctl status mongod （服务状态查看）
mongod.service - High-performance, schema-free document-oriented database
   Loaded: loaded (/usr/lib/systemd/system/mongod.service; enabled)
   Active: active (running) since 二 2015-09-15 11:05:33 CST; 1min 57s ago
  Process: 19181 ExecStart=/usr/bin/mongod $OPTIONS run (code=exited, status=0/SUCCESS)
 Main PID: 19183 (mongod)
   CGroup: /system.slice/mongod.service
           └─19183 /usr/bin/mongod --quiet -f /etc/mongodb.conf run

9月 15 11:05:33 node-9.eayun.com mongod[19181]: about to fork child process, waiting until server is ready ...ons.
9月 15 11:05:33 node-9.eayun.com mongod[19181]: forked process: 19183
9月 15 11:05:33 node-9.eayun.com mongod[19181]: child process started successfully, parent exiting
9月 15 11:05:33 node-9.eayun.com systemd[1]: Started High-performance, schema-free document-oriented database.
Hint: Some lines were ellipsized, use -l to show in full.

```

####mongodb 监控

```
通过mongostat查看mongo数据库信息
[root@node-9 ~]# mongostat -u sa -p  （使用mongo用户与密码登录）
Enter password:
connected to: 127.0.0.1
insert  query update delete getmore command flushes mapped  vsize    res faults  locked db idx miss %     qr|qw   ar|aw  netIn netOut  conn        set repl       time
    *0     *0     *0     *0       0     1|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0    62b     3k    11 ceilometer  PRI   23:13:57
    *0     *0     *0     *0       2     4|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   594b     4k    11 ceilometer  PRI   23:13:58
    *0     *0     *0     *0       0     1|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0    62b     3k    11 ceilometer  PRI   23:13:59
    *0     *0     *0     *0       0     4|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   500b     4k    11 ceilometer  PRI   23:14:00
    *0     *0     *0     *0       1     1|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   109b     3k    11 ceilometer  PRI   23:14:01
    *0     *0     *0     *0       0     4|0       0  4.31g  9.27g    88m      0 local:0.0%          0       0|0     0|0   500b     4k    11 ceilometer  PRI   23:14:02
    *0     *0     *0     *0       2     1|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   156b     3k    11 ceilometer  PRI   23:14:03
    *0     *0     *0     *0       0     4|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   500b     4k    11 ceilometer  PRI   23:14:04
    *0     *0     *0     *0       0     1|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0    62b     3k    11 ceilometer  PRI   23:14:05
    *0     *0     *0     *0       1     4|0       0  4.31g  9.27g    88m      0  test:0.0%          0       0|0     0|0   547b     4k    11 ceilometer  PRI   23:14:06

通过serverStatus获取与的MongoDB服务器统计信息
[root@node-9 ~]# mongo 127.0.0.1:27017/admin -u sa -p
MongoDB shell version: 2.6.11
Enter password:
connecting to: 127.0.0.1:27017/admin
ceilometer:PRIMARY> db.runCommand({"serverStatus":1})
{
	"host" : "node-9",
	"version" : "2.6.11",
	"process" : "mongod",
	"pid" : NumberLong(23022),
	"uptime" : 66735,
	"uptimeMillis" : NumberLong(66735149),
	"uptimeEstimate" : 65503,
	"localTime" : ISODate("2015-09-15T03:19:43.174Z"),
	"asserts" : {
		"regular" : 0,
		"warning" : 0,
		"msg" : 0,
		"user" : 20,
		"rollovers" : 0
	},
	"backgroundFlushing" : {
		"flushes" : 1112,
		"total_ms" : 21903,
		"average_ms" : 19.696942446043167,
		"last_ms" : 124,
		"last_finished" : ISODate("2015-09-15T03:19:28.365Z")
	},
	"connections" : {
		"current" : 12,
		"available" : 807,
		"totalCreated" : NumberLong(6692)
	},


		"storage" : {
			"freelist" : {
				"search" : {
					"bucketExhausted" : NumberLong(0),
					"requests" : NumberLong(2),
					"scanned" : NumberLong(4)
				}
			}
		},
		"ttl" : {
			"deletedDocuments" : NumberLong(0),
			"passes" : NumberLong(1112)
		}
	},
	"ok" : 1
}
ceilometer:PRIMARY>
该收集中信息比齐全，包括网络，复制集，存储等，因信息较多，未全部贴出
```

####Mongo 导入/导出

 Mongodb支持数据导入与导出操作，通过使用`mongoexport、mongoimport`导出/导入

##### Mongodb导出数据

举例导出数据为csv格式：

```
[root@node-9 ~]#mongoexport -h 127.0.0.1 --port 27017 -u ceilometer -p password -d ceilometer -c message -f name,phone --csv -o /tmp/mongo/ceilometer.csv
```
导出后在/tmp/mongo/下能看到ceilometer.csv文件，该文件可以通过excel打开

参数说明：

|参数|说明|
|----|----|
|-h|使用的mongo服务主机地址|
|--port|连接mongo端口|
|-u ceilometer|使用ceilometer用户连接|
|-p password|ceilometer 用户密码|
|-d ceilometer|连接的数据库ceilometer|
|-c message|ceilometer数据库下collections 下的文档|
|-f name,phone| message文档的name列，phone列|
|--csv|导出格式为csv格式|
|-o /tmp/mongo/ceilometer.csv|导出的文件存放路径|

##### Mongodb导入数据

举例数据导入为csv格式：
```
[root@node-9 ~]#mongoimport -h 127.0.0.1 --port 27017 -u ceilometer -p password -d ceilometer -c message --type csv --headerline -file /tmp/mongo/ceilometer.csv
```
导入完成后，通过查询，检查数据是否导入成功

```
ceilometer:PRIMARY>use ceilometer;
ceilometer:PRIMARY>db.message.find();
ceilometer:PRIMARY> db.message.find();
{ "_id" : "01", "name" : "xiaoming", "phone" : "0808008" }
{ "_id" : ObjectId("55f7e7fc63cdd4c9cba3c934"), "dizhi" : "ChengDu", "gongsi" : "ChengDu Gongshi" }
```

Mongodb 还支持json文件格式的导入

```
[root@node-9 ~]#mongoimport -h 127.0.0.1 --port 27017 -u ceilometer -p password -d ceilometer -c message  -file /tmp/mongo/ceilometer.json
```

#### Mongo数据库备份与恢复

Mongodb 支持数据库备份与恢复操作，通过使用`mongodump、mongorestore`进行数据库备份与恢复操作

##### Mongodb 数据库备份

* 登录数据库主库并创建数据库

`ceilometer:PRIMARY>use ceilometer;` （创建ceilometer 数据库）

* 为ceilometer 数据库添加用户，权限为dbowner

`ceilometer:PRIMARY>db.addUser('ceilometer','password');`

* 使用ceilometer 用户登录，并在ceilometer 数据库中创建collection

`[root@node-9 ~]# mongo 127.0.0.1:27017/ceilometer -u ceilometer -p` （使用ceilometer 用户登录ceilometer数据库）

`ceilometer:PRIMARY>db.message.insert({'_id':'01','name':'xiaoming','phone':'0808008'});` （创建collection)

* 备份ceilometer 数据库
`[root@node-9 ~]#mongodump -h 127.0.0.1 --port 27017 -u ceilometer -p password -d ceilometer -o /tmp/data/ceilometer` (备份ceilometer数据库）

备份完成后在/tmp/data/ceilometer下将会看到备份文件：

```
[root@node-9 ~]#ls
message.bson  message.metadata.json  system.indexes.bson
```
查看到备份文件后，备份成功

##### Mongodb数据库恢复

* 登录数据库主库，删除掉ceilometer 数据库

查看数据库

```
ceilometer:PRIMARY>show dbs;
admin  0.078GB
ceilometer  0.078GB
local  4.076GB
test   0.078GB
```

删除数据库

```
ceilometer:PRIMARY>use ceilometer; (切换到ceilometer数据库）
ceilometer:PRIMARY>db.dropDatabase(); （删除数据库ceilometer）
ceilometer:PRIMARY>show dbs;
admin  0.078GB
local  4.076GB
test   0.078GB
```

数据库ceilometer已被删除

* 通过备份恢复ceilometer 数据库

`[root@node-9 ~]# mongorestore -h 127.0.0.1 --port 27017 -u ceilometer -p password --db ceilometer /tmp/data/ceilometer/* ` (恢复数据库）

* 使用ceilometer登录，查看ceilometer是否恢复

```
ceilometer:PRIMARY>show dbs;
admin  0.078GB
ceilometer  0.078GB
local  4.076GB
test   0.078GB
ceilometer:PRIMARY>use ceilometer;
ceilometer:PRIMARY> show collections; （查看ceilometer数据库中collection已恢复）
message
system.indexes
ceilometer:PRIMARY>db.message.find(); (查看message的信息）
{'_id':'01','name':'xiaoming','phone':'0808008'}
```

ceilometer 数据库恢复完成



