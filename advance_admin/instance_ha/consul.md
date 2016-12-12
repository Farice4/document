# 云主机高可用consul集群介绍

Consul是HashiCorp公司推出的开源工具, 实现分布式系统的服务发现与配置。

## Consul 角色分类与使用场景

> Consul 角色分类

* client 客户端

客户端, 无状态, 将 HTTP 和 DNS 接口请求转发给局域网内的服务端集群。

* server 服务端

服务端, 保存配置信息, 高可用集群, 在局域网内与本地客户端通讯, 通过广域网与其他数据中心通讯. 每个数据中心的 server 数量推荐为 3 个或是 5 个。

> Consul 使用场景

* 服务监控类场景

服务注册有点像OpenStack Keystone的Endpoints，可以通过API方式查询到所有服务的端点信息。
agent 可以根据需要，添加一个service，然后重启agent服务，通过rest api的方式连接8500端口就能进行查询服务 。

* 健康检查

健康检查的方法主要是通过运行一小段脚本的方式，根据运行的结果判断检查对象的健康状况。所以可以通过任意语言定义这个脚本，脚本运行将通过和consul执行的相同用户执行。

## Consul 集群日常维护

> Consul 文件与目录介绍

|名称|作用|
|-|-|
|/etc/consul/storage/consul.json|Consul集群配置文件, 记录加入的集群, 节点为server或client|
|/var/lib/consul|Consul 数据文件, 快照文件|

Consul 日志记录在/var/log/messages中

Consul 服务使用端口包括8300,8301,8302,8400,8500


> Consul 服务健康状态检测

* Consul 服务检测

Consul 服务检测方法

```
systemctl status consul (查看服务状态)
```

* Consul member状态检测

Consul member状态检测方法

```
# consul members -rpc-addr=172.16.102.3:8400  (通过consul命令调用consul的api来查询）
  Node               Address            Status  Type    Build  Protocol  DC
  node-15.eayun.com  172.16.102.3:8301  alive   server  0.6.3  2         br_storage
  node-17.eayun.com  172.16.102.5:8301  alive   server  0.6.3  2         br_storage
  node-18.eayun.com  172.16.102.6:8301  alive   server  0.6.3  2         br_storage
```
 Status分为alive(代表正常），left(代表服务从集群中离开，可能的原因分为服务停止，防火墙端口未打开)

* Consul Leader选举状态检测

Consul Leader 选举状态检测方法

```
  # consul info -rpc-addr=172.16.102.3:8400
  agent:
    check_monitors = 0
    check_ttls = 0
    checks = 0
    services = 1
  build:
  	    prerelease =
    revision = c933efde
    version = 0.6.3
  consul:
    bootstrap = false
    known_datacenters = 1
    leader = false
    server = true
  raft:
    applied_index = 63555
    commit_index = 63555
    fsm_pending = 0
    last_contact = 4.989924ms
    last_log_index = 63555
    last_log_term = 303
    last_snapshot_index = 57400
    last_snapshot_term = 303
    num_peers = 2
    state = Follower
    term = 303
  runtime:
    arch = amd64
    cpu_count = 12
    goroutines = 92
    max_procs = 12
    os = linux
    version = go1.5.3
  serf_lan:
    encrypted = false
    event_queue = 0
    event_time = 42
    failed = 0
    intent_queue = 0
    left = 0
    member_time = 390
    members = 3
    query_queue = 0
    query_time = 1
  serf_wan:
    encrypted = false
    event_queue = 0
    event_time = 1
    failed = 0
    intent_queue = 0
    left = 0
    member_time = 13
    members = 1
    query_queue = 0
    query_time = 1
```
其中看`consul:`下面的leader的参数，leader = false 或者 true值， 3台节点中必定有一个为leader = true,其它两个节点leader = false， consul集群才能正常工作，只有server角色才能参与leader选举。

## Consul 集群leader不能选举故障恢复

Consul 集群leader不能选举的情况出现在集群中低于2/3的server存在, server之间不能正常通信, 进行选举。

针对Consul 集群不能选举的情况提供两种恢复方法： 数据文件删除的方式恢复, 手动添加文件中数据方式恢复

* 数据文件删除恢复（以3台server节点举例)

```
删除/var/lib/consul/raft/下数据方法恢复
  1、 3台节点都停止正在运行的consul服务
  2、 3台节点都执行删除/var/lib/consul/raft/下的所有数据(数据已经不可用)
  3、 分别重启3台节点的consul 服务，服务启动完成后通过consul info -rpc-addr=xxxx:8400能够看到至少一台节点已经成为
      leader
```

* 手动添加文件中数据方式恢复

```
手动添加/var/lib/consul/raft/peers.json 文件中的数据
  1、 3台节点都停止正在运行的consul服务
  2、 3台节点都执行修改/var/lib/consul/raft/peers.json文件数据 (当leader无法选举时，文件中数据为null),修改内容如下：
  ["172.16.102.6:8300","172.16.102.5:8300","172.16.102.3:8300"]  (其中的ip地址通过/etc/consul/storage/consul.json的配置文件中获取真实的ip地址
  3、 启动其中的2台节点后查看/var/lib/consul/raft/peers.json文件中修改的数据是否存在，如果存在证明正常，否则修改不成功，
  启动第3台节点，启动完成后同样查看修改后的peers.json文件中内容是否与修改的一致，如果一致证明修改成功
  4、 通过consul info -rpc-addr=xxxx:8400能够看到至少一台节点已经成为leader
```
