Ceph 是分布式文件系统，以分布式的 RADOS 为基础，对外提供对象存储，块存储，和文件系统存储。在 EayunStack 中，只用到 Ceph 的块存储功能给 Cinder 和 glance 提供后端存储。一个 Ceph 主要分为以下几个部分

| 组件 | 功能 |
| ---- | ---- |
| Monitor | 维护整个集群的基础信息 |
| OSD | 处理物理存储的 IO |
| MDS | 为 CephFS 提供元数据服务 |
| RADOSGW | 提供类似与 S3 的对象存储的 http 接口 |
| Client | 访问 Ceph 集群 |

### Monitor
Ceph Monitor 维护了整个 Ceph 集群的基础信息，包括 osd 的节点/位置信息，Ceph 集群的基本配置信息，OSD map 和 PG map等。在 EayunStack 中，Ceph Monitor 运行在控制节点中。

### OSD
每一个 Ceph OSD 对应一个物理设备（硬盘，或者分区），处理数据读写，数据冗余等。生产环境至少得有两个 ceph-osd 服务运行来做数据冗余。在 EayunStack 中，该服务部署在每一个 ceph-osd 节点

### MDS
存储 Ceph FS 的元数据，提供元数据服务，利用该服务，Ceph 可以对外提供兼容 POSIX 的文件系统存储。目前 EayunStack 暂不需要提供该服务。

### RADOSGW
提供兼容亚马逊 S3 和 OpenStack Swift 的 Http 接口。目前 EayunStack 暂不需要提供该服务。

### Client
访问 Ceph 集群的工具集，包括各种库和程序。在 EayunStack 中，控制节点的 glance， cinder-volume 和计算节点的 nova-compute 里面都包含了该 Client。glance 通过该 Client 来实现镜像的上传下载，控制节点 的cinder-volume 通过该 Client 来实现卷的管理（创建/删除/快照等）。nova-compute 通过该 Client 实现虚拟机的 IO。
