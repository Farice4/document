# 集群服务日志

|组件|日志文件|内容|存放节点|
|----|----|--------|----------|
|rabbitmq|/var/log/rabbitmq/rabbit@<nodename>.log|存放rabbitmq-server当前节点服务日志。|控制节点|
|rabbitmq|/var/log/rabbitmq/shutdown_log|存放rabbitmq服务关闭时的日志信息。|控制节点|
|rabbitmq|/var/log/rabbitmq/startup_log|存放rabbitmq服务启动时的日志信息。|控制节点|
|rabbitmq|/var/log/rabbitmq/startup_err|存放rabbitmq服务启动时的出错日志信息。|控制节点|
|mysql|/var/log/mysqld.log|存放mariadb/mysql数据库相关日志信息。|控制节点|
|ceph|/var/log/ceph/ceph-mon.[HOSTNAME].log| 存放 ceph monitor 的日志|控制节点|
|ceph|/var/log/ceph/ceph-osd.[OSD_ID].log|存放ceph osd相关日志信息。|ceph osd 节点|
|ceph|/var/log/ceph/ceph.log|存放ceph存储一般日志信息。|控制节点|
|pacemaker|/var/log/pacemaker.log|存放集群服务pacemaker的日志信息。|控制节点|

