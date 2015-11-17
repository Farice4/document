# 高可用集群 #
高可用集群基于 Corosync 和 Pacemaker 实现。

Corosync 是 Pacemaker 的基础服务，完成集群通信、维护节点信息等。

Pacemaker 是高可用集群的核心组件，集群资源的创建和管理都由它完成。


# 负载均衡集群 #
负载均衡集群基于 HAPorxy 实现。

为了防止 HAProxy 自身出现单点故障，HAProxy 服务程序也是高可用集群其中的一个资源。

