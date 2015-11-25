# 监控对象

* 各 OpenStack 节点系统基本状态监控
    * CPU 利用率
    * MEM、SWAP 使用量
    * Process 数量
    * 登陆用户数
    * 磁盘空间使用量
    * 磁盘 IO
    * 网卡 IO
* OpenStack 各 Controller 节点上各组件服务状态监控
    * API 5XX 错误数量
    * API HTTP Response 时间及数量
    * API 可用性（如：nova api）
    * 服务运行状态（如：openstack-nova-scheduler）
    * 资源状态 （如：运行中的 instance 数量）
* 集群状态监控
    * RabbitMQ 集群状态
    * MySQL 集群状态
    * HAProxy 集群状态
    * Ceph 集群状态
* 附加服务监控
    * Apache 服务状态
    * Memcached 服务状态
