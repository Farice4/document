# 集群管理

EayunStack 平台通过为基础服务建立集群来尽可能地保证整个平台的可用性，已实现的基础服务集群包括：

| 基础服务 | 集群方式 |
|----------|----------|
| OpenStack 各组件 | Corosync + Pacemaker 以及 HAProxy |
| MySQL 数据库 | Corosync + Pacemaker 以及 Galera |
| RabbitMQ 消息队列 | Corosync + Pacemaker 以及 RabbitMQ cluster 、HA queues |
| Ceph 存储 | 原生的分布式集群 |
