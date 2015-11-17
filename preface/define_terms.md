# 术语定义

## 节点角色

EayunStack环境中的物理服务器节点分为以下几种角色。

|英文名称|中文名称|简介|
|----|----|----|
|fuel|Fuel节点|EayunStack部署服务器|
|controller|控制节点|承载OpenStack环境所有核心服务|
|compute|计算节点|负责承载虚拟机运行|
|mongo|Mongo节点|MongoDB数据库节点，为Ceilometer提供数据库服务|
|ceph-osd|Ceph-osd节点|Ceph集群存储节点|

## 网络角色

EayunStack环境中的网络分为以下几种角色。

|网络角色|简介|
|----|----|
|PXE|Fuel部署网络，所有OpenStack节点通过该网络实现网络安装|
|Management|承载Controller节点与其它节点之间的管理流量|
|Public|承载虚拟机与互联网上主机之间通信的网络流量|
|Private|承载虚拟机之间通信的网络流量|
|Storage|承载虚拟机与存储服务器之间数据传输的网络流量|
|Ceph Cluster|承载Ceph-osd节点间数据同步的网络流量|
