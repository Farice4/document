# 云主机高可用自动迁移介绍

云主机高可用自动迁移（autoevacuate）, 通过检测电源管理的状态, 当电源管理检测到计算节点宕机, 查询等待自动迁移的云主机, 完成云主机从故障节点撤离。



## 实现条件

* Consul 集群正常运行, Leader选举成功
* 电源管理配置IPMI自动认证
* 云主机存储方式为共享存储方式
* 云主机撤离时原计算节点必须关机


## 实现原理

```
1、 autoevacuate 在controller节点检测本节点consul集群是否为leader
2、 autoevacuate 检测compute 节点ipmi电源状态, 判断compute节点是否宕机
3、 autoevacuate 检测到compute节点宕机, autoevacuate 筛选进行自动迁移的云主机
4、 autoevacuate 执行云主机撤离, 撤离过程中云主机会根据nova 的调度自动选择新的计算节点
5、 撤离完成
```


## 日常维护

* 自动迁移文档介绍

|名称|作用|
|-|-|
|/etc/autoevacuate/evacuate.conf|autoevacuate配置文件|
|/var/log/autoevacuate/auto-novaevacuate.log|autoevacuate 日志文件|

* 服务状态检测

服务状态检测方法

```
# systemctl status eayunstack-auto-evacuate.service
```

* 日志检测

查看/var/log/autoevacuate/auto-novaevacuate.log

日志主要查看控制节点为leader的auto-novaevacuate.log日志, 日志中会记录检测的计算节点状态, 如果有自动迁移, 会记录那些云主机进行了撤离操作。
