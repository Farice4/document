### 服务管理
Ceph 的集群管理通过自身的冗余算法来实现，对服务的管理采用 service 来实现。

目前在 EayunStack 的环境中，monitor的服务如下：

| 组件 | 服务名 | 运行的节点 |
| --- | --- | --- |
| mon | mon.node-1 | node-1 |
| mon | mon.node-2 | node-2 |
| mon | mon.node-3 | node-3 |
| client | | 所有控制节点, 计算节点和osd节点|


osd 由于对应的是硬盘，用 ceph osd tree 来查找服务和服务名

#### 管理一个 Monitor

登录到 Montior 所在的节点，从上表找到服务名（比如mon.node-1）
```
# [root@node-1 ~]# service ceph status mon.node-1 ### 查看monitor状态
# [root@node-1 ~]# service ceph start mon.node-1 ### 启动monitor
# [root@node-1 ~]# service ceph stop mon.node-1 ### 停止monitor
# [root@node-1 ~]# service ceph restart mon.node-1 ### 重启monitor
```

#### 管理一个 OSD
用 ceph osd tree 查找到需要管理的osd的主机名，登录到该主机。
```
# [root@node-9 ~]# service ceph status osd.0 ### 查看osd.0状态
# [root@node-9 ~]# service ceph start osd.0 ### 启动osd.0
# [root@node-9 ~]# service ceph stop osd.0 ### 停止osd.0
# [root@node-9 ~]# service ceph restart osd.0 ### 重启osd.0
```

#### 管理集群中所有 Monitor
(FIXME)暂不支持，需要用户登录每一个 Monitor 节点去操作
#### 管理集群中所有 OSD
(FIXME)暂不支持, 需要用户登录每一个 osd 节点去操作
#### 管理整个集群
(FIXME)暂不支持
#### 管理一个主机下的所有 OSD
```
# [root@node-9 ~]# service ceph -a status osd ### 查看osd节点下所有osd状态
# [root@node-9 ~]# service ceph -a start osd ### 启动osd节点下所有osd状态
# [root@node-9 ~]# service ceph -a stop osd ### 停止osd节点下所有osd
# [root@node-9 ~]# service ceph -a restart osd ### 重启osd节点下所有osd
```

### 检查集群状态
登录任意一个 Ceph节点（monitor节点，osd节点，client节点）执行下面的命令查看整个集群的状态
```bash
[root@node-9 ~]# ceph -s
    cluster be2a0c73-4585-49c9-afdf-37f2c7a173d4
     health HEALTH_WARN 3264 pgs down; 3264 pgs peering; 3264 pgs stuck inactive; 3264 pgs stuck unclean; 301 requests are blocked > 32 sec
     monmap e4: 3 mons at {node-1=172.16.102.2:6789/0,node-2=172.16.102.3:6789/0,node-3=172.16.102.4:6789/0}, election epoch 50, quorum 0,1,2 node-1,node-2,node-3
     osdmap e781: 19 osds: 14 up, 14 in
      pgmap v70091: 3264 pgs, 6 pools, 29052 MB data, 4001 objects
            90409 MB used, 12938 GB / 13027 GB avail
                3264 down+peering
```
> ###### 注意
> 不要放过任何一个 **HEALTH_WARN**

### 检查 Monitor 状态
登录任意一个 Ceph节点（monitor节点，osd节点，client节点）执行下名的命令查看整个集群的状态
```
[root@node-8 ~]# ceph mon_status
{"name":"node-2","rank":1,"state":"peon","election_epoch":50,"quorum":[0,1,2],"outside_quorum":[],"extra_probe_peers":[],"sync_provider":[],"monmap":{"epoch":4,"fsid":"be2a0c73-4585-49c9-afdf-37f2c7a173d4","modified":"2015-04-13 19:38:58.324666","created":"0.000000","mons":[{"rank":0,"name":"node-1","addr":"172.16.102.2:6789\/0"},{"rank":1,"name":"node-2","addr":"172.16.102.3:6789\/0"},{"rank":2,"name":"node-3","addr":"172.16.102.4:6789\/0"}]}}
```

### 检查 OSD 状态
```bash
[root@foreman ~]# ceph osd tree
2015-02-03 16:50:26.007404 7f38d01a0700  0 -- :/1030516 >> 10.10.0.55:6789/0 pipe(0x7f38cc0276e0 sd=3 :0 s=1 pgs=0 cs=0 l=1 c=0x7f38cc027970).fault
# id    weight  type name       up/down reweight
-1      12      root default
-2      0               host ceph1
-3      4               host foreman
0       1                       osd.0   up      1
3       1                       osd.3   up      1
8       1                       osd.8   up      1
9       1                       osd.9   up      1
-4      4               host ceph2
1       1                       osd.1   up      1
4       1                       osd.4   up      1
10      1                       osd.10  up      1
11      1                       osd.11  up      1
-5      4               host ceph3
2       1                       osd.2   up      1
5       1                       osd.5   up      1
6       1                       osd.6   up      1
7       1                       osd.7   up      1
```
> ###### 注意
> 不为 up 的状态就说明该 osd.0 出错了


