# 配置Ceph Cluster网络

默认配置中，Ceph集群的public/mon/cluster网络全部使用Fuel管理界面中节点网卡配置中所指定的Storage网络，在生产环境中，我们需要修改环境配置，使Ceph集群的cluster网络对应一个独立的EayunStack网络(Ceph Cluster Network)，该网络对应独立的网卡。

### 配置流程

* 登录Fuel节点

* 确认所要修改的环境的ID，命令示例：

```
[fuel]# fuel environment --name EayunStack
id | status      | name       | mode       | release_id | changes                                                                                | pending_release_id
---|-------------|------------|------------|------------|----------------------------------------------------------------------------------------|-------------------
1  | operational | EayunStack | ha_compact | 1          | [{u'node_id': 1, u'name': u'disks'}, {u'node_id': 2, u'name': u'disks'}]             | None              
```

如上示例，“name”为“EayunStack”的条目为我们本次所部署的环境信息，该条目第1列为我们要确认的环境ID，该示例中环境ID为”1“。

* 确认ceph-osd节点的节点ID

```
[fuel]# fuel node
id | status | name             | cluster | ip            | mac               | roles      | pending_roles | online | group_id
---|--------|------------------|---------|---------------|-------------------|------------|---------------|--------|---------
4 | ready  | Untitled (63:f1) | 3       | 172.16.100.11 | 42:94:8b:33:51:4f | mongo      |               | True   | 3       
6 | ready  | Untitled (a9:54) | 3       | 172.16.100.10 | 1a:b1:06:47:38:42 | ceph-osd   |               | True   | 3       
1 | ready  | Untitled (c6:52) | 3       | 172.16.100.4  | 5a:61:ec:05:ef:42 | controller |               | True   | 3       
7 | ready  | Untitled (09:e6) | 3       | 172.16.100.9  | 22:39:da:0b:99:4f | ceph-osd   |               | True   | 3       
5 | ready  | Untitled (d5:27) | 3       | 172.16.100.6  | 76:76:dc:95:a1:4c | compute    |               | True   | 3       
2 | ready  | Untitled (e3:52) | 3       | 172.16.100.7  | 02:21:31:20:54:40 | controller |               | True   | 3       
3 | ready  | Untitled (e3:b7) | 3       | 172.16.100.5  | de:68:b2:ac:fc:4a | controller |               | True   | 3       
8 | ready  | Untitled (10:9e) | 3       | 172.16.100.8  | 52:b7:83:88:36:49 | ceph-osd   |               | True   | 3       

```

如上示例，该环境中ceph-osd节点的id分别为**6**,**7**,**8**

* 配置ceph cluster网络

```
[fuel]# eayunstack fuel ceph_cluster_network --env 1 --cidr 172.16.200.0/24 --nic_mappings 6:eth4,7:eth4,8:eth4
```

* 配置ceph cluster网络使用网卡bonding

  ```
  [fuel]# eayunstack fuel ceph_cluster_network --env 1 --cidr 172.16.200.4/24 --nic_mappings 6:eth4+eth5,7:eth4+eth5,8:eth4+eth5 [--bond_mode active-backup/balance-slb/lacp-balance-tcp]
  ```

  说明：
    * 以上例子表示几个节点同时将 eth4 和 eth5 网卡作为 bonding 负载 ceph cluster 网络
    * bond_mode 有三种模式可选，默认为 active-backup
