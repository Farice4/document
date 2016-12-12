# 云主机高可用

当云主机所在的计算（compute)节点出现宕机, 云主机将会通过自动迁移的方式将云主机撤离至同一个环境中的其它计算节点。


## 云主机高可用实现方式

* Consul 集群提供leader选举

* EayunStack autoevacuate提供云主机计算节点电源管理检测与云主机自动迁移


> 重点

eayunstack autoevacuate依赖Consul集群运行, 如果Consul 集群不能提供正常服务, 不能进行leader选举, auto-evacuate将不能提供云主机电源管理检测与云主机自动迁移。
