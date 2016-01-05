# 灾难场景

```
* 场景N： * * *

* 故障等级：

* 现象描述：

* 故障模拟方案

* 故障原因：
1）
2）
3）
。。。
N）

* 恢复方案：
1）排查方法
2）解决方法

* 预计故障恢复时间

  排查&解决问题所用的总时间
```

#### 场景 No.1：某个租户所有虚拟机无法连接外网

* 故障等级：

* 现象描述：

  * 登录某个租户的任意一台虚拟机，ping公网任意一个IP地址，不通。

* 故障模拟方案

* 故障原因：

  * 路由未连接外网；
  * 或内网、外网的端口状态为 **DOWN**；

* 恢复方案：

  * 排查方法
    
    * 登录到 Controller 节点，执行命令 `neutron router-show <router_name>`，其中 \<router\_name\> 为对应的路由名称，查看该路由的状态；
    * 也可以登录 dashboard 后，点击 【Project】 -> 【Network】 -> 【Routers】，在右侧列出的路由列表中选择对应的路由进入详细页面，查看接口的状态。
    
  * 解决方法

    1. 登录 Controller 节点；
    1. 执行命令 `neutron router-update <router_name> --admin_state_up True`，恢复路由状态为 **UP**；
    1. 执行命令 `neutron router-show <router_name>` 确认路由状态是否恢复正常。

* 预计故障恢复时间

===

#### 场景 No.2：某些虚拟机无法连接外网

* 故障等级：

* 现象描述：

  * 环境中的虚拟机，ping 公网任意一个 IP 地址，不通，ping 虚拟机路由，不通。

* 故障模拟方案

* 故障原因：

  1. 环境里所有的 neutron-l3-agent 都与 neutron-server 断开连接；
  1. 而 neutron-server 先发现了其中一台节点（node-A）上的 l3-agent 断开，则将该节点上的路由迁移到其他节点上；
  1. 此时恰好发现其他节点（如 node-B）的 l3-agent 还连接着（其实已经断开连接，只是数据库还没来得及更新状态），于是将 node-A 上的路由全部迁移到 node-B 上；
  1. 迁移时需要将 internal port 绑定到 node-B 上，此时恰好 node-B 更新了状态，l3-agent 连接断开，绑定失败。

* 恢复方案：

  * 排查方法
    
    1. 登录到 Controller 节点，执行 `eayunstack doctor net vrouter`，提示：`[ ERROR ] (controller) (node-5.eayun.com): status of port network:router_interface[1e123c6d-a14d-40ae-b99f-82325a23e44b] on node-5.eayun.com is down`，找到出现问题的端口；
    1. 执行命令查看该端口的情况，看到端口状态为 **DOWN** 且 binding:vif_type 为 **binding_failed**：

        ```
        (controller)# neutron port-show 1e123c6d-a14d-40ae-b99f-82325a23e44b
        +-----------------------+--------------------------------------------------------------------------------+
        | Field                 | Value                                                                          |
        +-----------------------+--------------------------------------------------------------------------------+
        | admin_state_up        | True                                                                           |
        | allowed_address_pairs |                                                                                |
        | binding:host_id       | node-5.eayun.com                                                               |
        | binding:profile       | {}                                                                             |
        | binding:vif_details   | {}                                                                             |
        | binding:vif_type      | binding_failed                                                                 |
        | binding:vnic_type     | normal                                                                         |
        | device_id             | 0ec8c1ec-c52b-444e-ba84-593830b18cf6                                           |
        | device_owner          | network:router_interface                                                       |
        | extra_dhcp_opts       |                                                                                |
        | fixed_ips             | {"subnet_id": "0567bc8b-ace4-415b-b1be-b3afcc4f386c", "ip_address": "5.5.5.1"} |
        | id                    | 1e123c6d-a14d-40ae-b99f-82325a23e44b                                           |
        | mac_address           | fa:16:3e:83:6f:ac                                                              |
        | name                  |                                                                                |
        | network_id            | 592086b8-765f-429d-ab33-27a07fc71784                                           |
        | security_groups       |                                                                                |
        | status                | DOWN                                                                           |
        | tenant_id             | 722ab8ce061248d18e79d83e2a746249                                               |
        +-----------------------+--------------------------------------------------------------------------------+
        ```
    
  * 解决方法

    1. 登录 Controller 节点；
    1. 查看 neutron-l3-agent 列表，执行命令：

        ```
        (controller)# neutron agent-list | grep l3
        | 00c94036-fc5a-4105-82f5-1ffd445e842f | L3 agent           | node-5.eayun.com  | :-)   | True           | neutron-l3-agent          |
        | 95b823d6-a503-4c80-aa95-75f35770bc6b | L3 agent           | node-8.eayun.com  | :-)   | True           | neutron-l3-agent          |
        | ccbd988b-8ca2-4e90-88c3-0fb5bf601d0d | L3 agent           | node-6.eayun.com  | :-)   | True           | neutron-l3-agent          |
        ```
    1. 记下 node-5.eayun.com 的 agent-id（因为该路由运行在 node-5.eayun.com 上）；
    1. 将路由从这个 l3-agent 中移除：`neutron l3-agent-router-remove \<agent_id\> \<router_id\>`，即 `neutron l3-agent-router-remove 00c94036-fc5a-4105-82f5-1ffd445e842f 0ec8c1ec-c52b-444e-ba84-593830b18cf6` （在 port-show 列出的信息中，device_id 指的是该端口对应的路由）；
    1. 将路由添加到另一个 l3-agent 中：`neutron l3-agent-router-add \<agent_id\> \<router_id\>`，如添加到 node-6.eayun.com 的 l3-agent 中：`neutron l3-agent-router-add ccbd988b-8ca2-4e90-88c3-0fb5bf601d0d 0ec8c1ec-c52b-444e-ba84-593830b18cf6`；
    1. 重新查看端口状态，确认问题解决：

        ```
        (controller)# neutron port-show 1e123c6d-a14d-40ae-b99f-82325a23e44b
        +-----------------------+--------------------------------------------------------------------------------+
        | Field                 | Value                                                                          |
        +-----------------------+--------------------------------------------------------------------------------+
        | admin_state_up        | True                                                                           |
        | allowed_address_pairs |                                                                                |
        | binding:host_id       | node-6.eayun.com                                                               |
        | binding:profile       | {}                                                                             |
        | binding:vif_details   | {"port_filter": true, "ovs_hybrid_plug": true}                                 |
        | binding:vif_type      | ovs                                                                            |
        | binding:vnic_type     | normal                                                                         |
        | device_id             | 0ec8c1ec-c52b-444e-ba84-593830b18cf6                                           |
        | device_owner          | network:router_interface                                                       |
        | extra_dhcp_opts       |                                                                                |
        | fixed_ips             | {"subnet_id": "0567bc8b-ace4-415b-b1be-b3afcc4f386c", "ip_address": "5.5.5.1"} |
        | id                    | 1e123c6d-a14d-40ae-b99f-82325a23e44b                                           |
        | mac_address           | fa:16:3e:83:6f:ac                                                              |
        | name                  |                                                                                |
        | network_id            | 592086b8-765f-429d-ab33-27a07fc71784                                           |
        | security_groups       |                                                                                |
        | status                | ACTIVE                                                                         |
        | tenant_id             | 722ab8ce061248d18e79d83e2a746249                                               |
        +-----------------------+--------------------------------------------------------------------------------+
        ```

    > #### 重要：
    > * 只有当 Internal interface 显示为 **DOWN** 且端口 **binding_failed** 时，才符合这个情况；
    > * 将路由从 l3-agent 移除后，必需添加到**另一个** l3-agent 上，问题才能修复，添加到同一个 l3-agent 中无法修复问题。

* 预计故障恢复时间

===

#### 场景 No.3：虚拟机无法通过DHCP方式获取IP地址

* 故障等级：

* 现象描述：

  * 登录虚拟机，查看IP地址等网络信息，处于未配置状态。

* 故障模拟方案

* 故障原因：

  * DHCP 代理故障。

* 恢复方案：

  * 排查方法
    
    * 登录 Controller 节点，执行命令 `pcs resource`，查看 p_neutron-dhcp-agent 服务的状态，如果为 "Stopped"，说明 DHCP 代理故障。
    
  * 解决方法

    1. 登录到 Controller 节点，执行命令 `pcs resource enable p_neutron-dhcp-agent`，重启 dhcp-agent 服务；
    1. 打开云主机控制台，重启网络或通过 DHCP 获取 IP 后，执行 `ip addr` 查看 IP 地址信息，确认能够获取到地址。

* 预计故障恢复时间

===

#### 场景 No.4：所有租户的所有虚拟机无法连接外网

* 故障等级：

* 现象描述：

  * 登录所有租户的任意一台虚拟机，ping公网任意一个IP地址，不通。

* 故障模拟方案

* 故障原因：

  * 环境外网被切断。

* 恢复方案：

  * 排查方法
    
    * 检查外网网络是否正常。
    
  * 解决方法

    * 恢复外网网络。

* 预计故障恢复时间

* **备注**：

  目前的环境暂时无法模拟。

===

#### 场景 No.5：同一租户下两台虚拟机网络流量很高，导致所有租户的网络受到影响

* 故障等级：

* 现象描述：

  * 登录任意一台虚拟机，ping其它节点或外网延迟非常大，丢包率非常高。

* 故障模拟方案

* 故障原因：

  * 环境中某一台或多台虚拟机向外发送大量数据包，占用了大量网络带宽。

* 恢复方案：

  * 排查方法
    
    * 
    
  * 解决方法

    * 

* 预计故障恢复时间

* **备注**：

  目前的环境暂时无法模拟。

===

#### 场景 No.6：虚拟机限速时，在该虚拟机向外发包，其它租户的虚拟机的网络受到影响

* 故障等级：

* 现象描述：

  * 登录任意一台虚拟机，ping其它节点或外网延迟非常大，丢包率非常高。

* 故障模拟方案

* 故障原因：

  * 环境中某一台或多台被限速的虚拟机向外发送大量的数据包，占用了大量带宽。

* 恢复方案：

  * 排查方法
    
    * 
    
  * 解决方法

    * 

* 预计故障恢复时间

* **备注**：

  目前的环境暂时无法模拟。

===

#### 场景 No.7(已测试通过)：DashBoard无法访问，监控系统发现大于或等于 N/2 台 Controller 节点宕机（N为Controller节点总数）

* 故障等级：

* 现象描述：

  * DashBoard无法访问，监控系统发现大于或等于 N/2 台 Controller 节点宕机（N为Controller节点总数）

* 故障模拟方案：

  同时切断大于或等于 N/2 台 Controller 节点的电源。

* 故障原因：

  * 环境中 N/2 台 Controller 节点宕机（N为Controller节点总数）

* 恢复方案：

  * 排查方法
    
    * 根据监控系统确认当前运行的 Controller 节点数量是否小于 N/2 台（N为Controller节点总数）
    
  * 解决方法

    * 重新启动所有宕机的 Controller 节点
    * 等待大约5分钟后，在任意一台 Controller 节点上使用 ```# pcs resource```命令确认 pacemaker 集群中所有资源处于 Started 状态。如下所示：
    ```
    # pcs resource
     vip__public	(ocf::mirantis:ns_IPaddr2):	Started 
     Clone Set: clone_ping_vip__public [ping_vip__public]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     vip__management	(ocf::mirantis:ns_IPaddr2):	Started 
     Clone Set: clone_p_openstack-heat-engine [p_openstack-heat-engine]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     p_openstack-ceilometer-central	(ocf::mirantis:ceilometer-agent-central):	Started 
     p_openstack-ceilometer-alarm-evaluator	(ocf::mirantis:ceilometer-alarm-evaluator):	Started 
     Clone Set: clone_p_neutron-openvswitch-agent [p_neutron-openvswitch-agent]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     p_neutron-dhcp-agent	(ocf::mirantis:neutron-agent-dhcp):	Started 
     Clone Set: clone_p_neutron-metadata-agent [p_neutron-metadata-agent]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     Clone Set: clone_p_neutron-l3-agent [p_neutron-l3-agent]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     Clone Set: clone_p_mysql [p_mysql]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     Clone Set: clone_p_rabbitmq-server [p_rabbitmq-server]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     Clone Set: clone_p_haproxy [p_haproxy]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
     Clone Set: clone_p_neutron-lbaas-agent [p_neutron-lbaas-agent]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
    ```
    * 如有某些资源在某个节点处于 Stopped 状态，可使用```pcs resource ban 资源名 节点主机名```及```pcs resource clear 资源名 节点主机名```尝试启动该资源。
    * 如果某些资源在某个节点处于 unmanaged 状态，可登陆该节点，使用```pcs resource cleanup 资源名```尝试重新启动该资源。

* 预计故障恢复时间

===

#### 场景 No.8（已测试通过）：环境中多台云主机(boot from volume)宕机，监控系统发现 Compute 节点宕机

* 故障等级：

* 现象描述：

  * 环境中多台云主机宕机，监控系统发现 Compute 节点宕机

* 故障模拟方案

  切断一台 Compute 节点电源。

* 故障原因：

  * Compute 节点宕机

* 恢复方案：

  * 排查方法
    
    * 通过监控系统确认 Compute 节点已经宕机
    
  * 解决方法
 
    * 执行命令```nova host-evacuate 宕机节点主机名```，将运行在宕机的 Compute 节点上的所有云主机迁移到其他节点
    * 确认所有虚拟机已迁移成功

*** 备注： ***测试时boot-from-image类型的虚拟机迁移失败，原因为compute节点断电后ceph中存在关于该虚拟机的rbd client。该类型的虚拟机迁移成功的前提是在compute节点断电后约10分钟后，确认对应 rbd client消失后，执行迁移即可成功。该问题还需要讨论研究。

* 预计故障恢复时间

===

#### No.9（已测试通过）：某些虚拟机出现磁盘I/O错误

* 故障等级：

* 现象描述：

  * 某些虚拟机出现磁盘I/O错误

* 故障模拟方案：

  该故障被恢复的前提为环境中存在一份磁盘（rbd volume）未损坏时的备份，使用备份恢复磁盘。备份磁盘的步骤：
  * 为虚拟机磁盘卷制作快照
  ```
  (controller)# rbd -p volumes snap create \
  --snap volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2-snap \
  --image volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2
  ```
  * 将快照导出到备份文件中
  ```
  (controller)# rbd -p volumes export \
  --image volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2@volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2-snap \
  volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2-backup
  ```
  * 删除快照
  ```
  (controller)# rbd -p volumes snap rm \
  --image volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2 \
  --snap volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2-snap
  ```

* 故障原因：

  * 虚拟机的磁盘（rbd volume）损坏。

* 恢复方案：

  * 排查方法
    
    * 登陆虚拟机中，执行命令或对某些文件进行读写时系统报I/O错误。
    
  * 解决方法

    * 关闭虚拟机
    * 删除虚拟机磁盘卷
    ```
    (controller)# rbd -p volumes rm --image volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2
    ```
    * 从备份文件中导入卷
    ```
    (controller)# rbd -p volumes import \
    --image-format 2 \
    --order 22 \
    --path volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2-backup \
    --image volume-038a66c5-fa63-4edb-a54e-b3f282ecb9e2
    ```
    * 启动虚拟机

* 预计故障恢复时间

===

#### 场景 No.10（已测试通过）：Ceilometer 服务不可用，报数据库连接错误。

* 故障等级：

* 现象描述：

  * Ceilometer 服务不可用，报数据库连接错误。

* 故障模拟方案：

  切断 Mongo 节点电源

* 故障原因：

  * Mongo节点宕机

* 恢复方案：

  * 排查方法
    
    * 通过监控系统确认 Mongo 节点已经宕机
    * 登陆任意 Controller 节点，在 /var/log/ceilometer/api.log 日志中，发现类似如下错误日志：
    ```
    2015-08-10 10:07:30.347 2816 ERROR wsme.api [-] Server-side error: "could not connect to 172.16.101.11:27017: [Errno 113] EHOSTUNREACH". Detail: 
Traceback (most recent call last):

  File "/usr/lib/python2.7/site-packages/wsmeext/pecan.py", line 77, in callfunction
    result = f(self, *args, **kwargs)

  File "/usr/lib/python2.7/site-packages/ceilometer/api/controllers/v2.py", line 2226, in get_all
    for m in pecan.request.alarm_storage_conn.get_alarms(**kwargs)]

  File "/usr/lib/python2.7/site-packages/ceilometer/alarm/storage/pymongo_base.py", line 200, in _retrieve_alarms
    for alarm in alarms:

  File "/usr/lib64/python2.7/site-packages/pymongo/cursor.py", line 814, in next
    if len(self.__data) or self._refresh():

  File "/usr/lib64/python2.7/site-packages/pymongo/cursor.py", line 763, in _refresh
    self.__uuid_subtype))

  File "/usr/lib64/python2.7/site-packages/pymongo/cursor.py", line 700, in __send_message
    **kwargs)

  File "/usr/lib64/python2.7/site-packages/pymongo/mongo_client.py", line 985, in _send_message_with_response
    sock_info = self.__socket()

  File "/usr/lib64/python2.7/site-packages/pymongo/mongo_client.py", line 720, in __socket
    host, port = self.__find_node()

  File "/usr/lib64/python2.7/site-packages/pymongo/mongo_client.py", line 713, in __find_node
    raise AutoReconnect(', '.join(errors))

AutoReconnect: could not connect to 172.16.101.11:27017: [Errno 113] EHOSTUNREACH
    ```
    
  * 解决方法

    * 重启 Mongo 节点，确认 mongod 服务正常运行

* 预计故障恢复时间

===

#### 场景 No.11（已测试通过）：所有 OpenStack 节点NTP同步失败

* 故障等级：

* 现象描述：

  * 所有 OpenStack 节点NTP同步失败。

* 故障模拟方案:

  停止 Fuel 节点 NTP 服务```# systemctl stop ntpd```

* 故障原因：

  * Fuel 节点 NTP 服务停止，无法向 OpenStack 节点提供 NTP 服务

* 恢复方案：

  * 排查方法
    
    * 登陆 Fuel 节点，查看 NTP 服务状态```systemctl status ntpd```
    
  * 解决方法

    * 重启 NTP 服务，使 NTP 服务恢复 active (running) 状态。

* 预计故障恢复时间

#### 场景 No.12（无经典场景，暂时保留）：MySQL 集群故障

* 故障等级：

* 现象描述：

  * 任意 OpenStack 组件连接数据库失败，日志中报数据库连接错误。

* 故障模拟方案：

  

* 故障原因：

  * 

* 恢复方案：

  * 排查方法
    
    * 
    
  * 解决方法

    * 

* 预计故障恢复时间

===

#### 场景 No.13（已测试通过）：RabbitMQ 集群故障

* 故障等级：

* 现象描述：

  * 任意 OpenStack 组件连接 RabbitMQ 失败，日志中报 rabbitmq 连接错误。

* 故障模拟方案：

  通过人为切断某些 Controller 节点的管理网络来破坏 RabbitMQ 集群内部通讯，从而打乱集群。

* 故障原因：

  * RabbitMQ 集群各节点间无法正常通讯。

* 恢复方案：

  * 排查方法
    
    * 在 Controller 节点查看 RabbitMQ 集群状态，执行命令 `pcs resource`，如果集群状态正常，将看到集群内的所有节点均在 "Started" 列表中，输出如下：

    ```
    # pcs resource
    ...
     Clone Set: clone_p_rabbitmq-server [p_rabbitmq-server]
         Started: [ node-5.eayun.com node-6.eayun.com node-8.eayun.com ]
    ...
    ```
    * 如果想要进一步查看集群状态，可以在 Controller 节点执行命令 `rabbitmqctl cluster_status`，集群状态正常时，所有集群内的节点均在 "running_nodes" 列表中，输出如下：

    ```
    # rabbitmqctl cluster_status
    Cluster status of node 'rabbit@node-6' ...
    [{nodes,[{disc,['rabbit@node-5','rabbit@node-6','rabbit@node-8']}]},
     {running_nodes,['rabbit@node-5','rabbit@node-8','rabbit@node-6']},
     {cluster_name,<<"rabbit@node-6.eayun.com">>},
     {partitions,[]}]
    ...done.
    ```

    > ###### 说明：
    > 当集群状态异常时，通过上述命令查看集群状态，可以看到故障节点不在 "Started" 或 "running_nodes" 列表中。

  * 解决方法

    * 判断哪台 Controller 节点故障，恢复故障节点的网络环境后，重启其上的 RabbitMQ 服务，执行命令 `pcs resource ban p_rabbitmq-server <node_name>` 及 `pcs resource clear p_rabbitmq-server <node_name>`；
    * 集群重启后，重启故障节点上的所有 OpenStack 服务：

    ```
    # pcs resource disable/enable clone_p_neutron-lbaas-agent
    # pcs resource disable/enable clone_p_neutron-lbaas-agent
    # pcs resource disable/enable clone_p_neutron-l3-agent
    # pcs resource disable/enable clone_p_neutron-metadata-agent
    # pcs resource disable/enable p_neutron-dhcp-agent
    # pcs resource disable/enable clone_p_neutron-openvswitch-agent
    # pcs resource disable/enable clone_p_openstack-heat-engine
    # openstack-service restart
    ```

* **备注**：

  RabbitMQ 集群故障的原因可能很多(管理网络断开是其中之一)，但最终表现为集群异常。故障原因可能包括：

  * 某台 Controller 节点与其他 Controller 节点无法通信(管理网络异常)；
  * RabbitMQ 相关进程异常；
  * 。。。

* 预计故障恢复时间



