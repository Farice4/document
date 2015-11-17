EayunStack 基础服务集群中各服务组件的管理都是高可用集群完成的，因此常用操作都针对高可用集群资源。

关于基础服务集群中高可用集群资源以及负载均衡集群服务配置情况请参见：

[EayunStack 高可用集群中的资源](high_availability.md#eayunstack_ha_resources_list)

[EayunStack 负载均衡集群中的服务](load_balance.md#eayunstack_lb_services_list)

# vip\_\_public
`vip__public` 对应 EayunStack 平台的 Public 网络使用的 IP 地址。在高可用集群中属于一个普通资源，整个集群中只运行一个实例（即同一时刻只在集群之中的一台节点服务上运行）。

`vip__public` 启动有 2 个先决条件：
1. 该节点服务器能够通过 ping 访问安装 EayunStack 平台时指定的 Public 网络的网关。（即该节点服务器网络连通正常。）
1. 该节点的 `clone_p_haproxy` 资源已经启动。

## vip\_\_public 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

如果要确认 `vip__public` 当前的运行节点，可以使用下面的命令：

    # crm_resource --locate --resource vip__public

## 启动/停止 vip\_\_public
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都会自动启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `vip__public` 资源，使用命令：

    # pcs resource disable vip__public

上述命令执行之后，`vip__public` 就被禁用了，无法在任何一台节点服务器上启动。

要恢复被禁用的 `vip__public` 资源，使用命令：

    # pcs resource enable vip__public

## 迁移 vip\_\_public 资源
要将 `vip__public` 资源迁移到另一台节点服务器，使用命令：

    # pcs resource move vip__public

`vip__public` 资源迁移的目标节点服务器由高可用集群自行决定。

如果要指定迁移的目标节点服务器（如：当前运行节点为 node-1 ，要迁移到 node-2 ），使用命令：

    # pcs resource move vip__public node-2

# vip\_\_management
`vip__management` 对应 OpenStack 平台的 Management 网络使用的 IP 地址。在高可用集群中属于一个普通资源，整个集群中只运行一个实例（即同一时刻只在集群之中的一台节点服务上运行）。

`vip__management` 启动有 1 个先决条件：
1. 该节点的 `clone_p_haproxy` 资源已经启动。

## vip\_\_management 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

如果要确认 `vip__management` 当前的运行节点，可以使用下面的命令：

    # crm_resource --locate --resource vip__management

## 启动/停止 vip\_\_management
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `vip__management` 资源，使用命令：

    # pcs resource disable vip__management

上述命令执行之后，`vip__management` 就被禁用了，无法在任何一台节点服务器上启动。

要恢复被禁用的 `vip__management` 资源，使用命令：

    # pcs resource enable vip__management

## 迁移 vip\_\_management 资源
要将 `vip__management` 资源迁移到另一台节点服务器，使用命令：

    # pcs resource move vip__management

`vip__management` 资源迁移的目标节点服务器由高可用集群自行决定。

如果要指定迁移的目标节点服务器（如：当前运行节点为 node-1 ，要迁移到 node-2 ），使用命令：

    # pcs resource move vip__management node-2

# p\_openstack-heat-engine
`p_openstack-heat-engine` 用于管理 heat-engine 服务。由于 heat-engine 可以同时启动多个实例，因此，在 `p_openstack-heat-engine` 基础之上实现 `clone_p_openstack-heat-engine` ,即所有 HA 集群节点服务器都会启动 1 个 `p_openstack-heat-engine` 实例。

`p_openstack-heat-engine`/`clone_p_openstack-heat-engine` 不依赖高可用集群中的其它资源。

## p\_openstack-heat-engine 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_openstack-heat-engine` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_openstack-heat-engine` 实例。

## 启动/停止 p\_openstack-heat-engine
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_openstack-heat-engine` 资源，由于 `clone_p_openstack-heat-engine` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_openstack-heat-engine

上述命令执行之后，`clone_p_openstack-heat-engine` 就被禁用了，此时所有节点服务服务上都无法运行 `p_openstack-heat-engine` 实例。

要恢复被禁用的 `clone_p_openstack-heat-engine` 资源，使用命令：

    # pcs resource enable clone_p_openstack-heat-engine

## 临时停止某一节点上的 p\_openstack-heat-engine 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_openstack-heat-engine` 实例停用，使用命令：

    # pcs resource ban p_openstack-heat-engine node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_openstack-heat-engine` 实例了。

之后，如果要恢复在 node-2 节点上启动 `p_openstack-heat-engine` 实例，则使用命令：

    # pcs resource clear p_openstack-heat-engine node-2

# p\_openstack-ceilometer-central
`p_openstack-ceilometer-central` 用于管理 ceilometer-central 服务。。在高可用集群中属于一个普通资源，整个集群中只运行一个实例（即同一时刻只在集群之中的一台节点服务上运行）。

`p_openstack-ceilometer-central` 不依赖高可用集群中的其它资源。

## p\_openstack-ceilometer-central 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

如果要确认 `p_openstack-ceilometer-central` 当前的运行节点，可以使用下面的命令：

    # crm_resource --locate --resource p_openstack-ceilometer-central

## 启动/停止 p\_openstack-ceilometer-central
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_openstack-ceilometer-central` 资源，使用命令：

    # pcs resource disable p_openstack-ceilometer-central

上述命令执行之后，`p_openstack-ceilometer-central` 就被禁用了，无法在任何一台节点服务器上启动。

要恢复被禁用的 `p_openstack-ceilometer-central` 资源，使用命令：

    # pcs resource enable p_openstack-ceilometer-central

## 迁移 p\_openstack-ceilometer-central 资源
要将 `p_openstack-ceilometer-central` 资源迁移到另一台节点服务器，使用命令：

    # pcs resource move p_openstack-ceilometer-central

`p_openstack-ceilometer-central` 资源迁移的目标节点服务器由高可用集群自行决定。

如果要指定迁移的目标节点服务器（如：当前运行节点为 node-1 ，要迁移到 node-2 ），使用命令：

    # pcs resource move p_openstack-ceilometer-central node-2

# p\_openstack-ceilometer-alarm-evaluator
`p_openstack-ceilometer-alarm-evaluator` 用于管理 ceilometer-alarm-evaluator 服务。。在高可用集群中属于一个普通资源，整个集群中只运行一个实例（即同一时刻只在集群之中的一台节点服务上运行）。

`p_openstack-ceilometer-alarm-evaluator` 不依赖高可用集群中的其它资源。

## p\_openstack-ceilometer-alarm-evaluator 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

如果要确认 `p_openstack-ceilometer-alarm-evaluator` 当前的运行节点，可以使用下面的命令：

    # crm_resource --locate --resource p_openstack-ceilometer-alarm-evaluator

## 启动/停止 p\_openstack-ceilometer-alarm-evaluator
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_openstack-ceilometer-alarm-evaluator` 资源，使用命令：

    # pcs resource disable p_openstack-ceilometer-alarm-evaluator

上述命令执行之后，`p_openstack-ceilometer-alarm-evaluator` 就被禁用了，无法在任何一台节点服务器上启动。

要恢复被禁用的 `p_openstack-ceilometer-alarm-evaluator` 资源，使用命令：

    # pcs resource enable p_openstack-ceilometer-alarm-evaluator

## 迁移 p\_openstack-ceilometer-alarm-evaluator 资源
要将 `p_openstack-ceilometer-alarm-evaluator` 资源迁移到另一台节点服务器，使用命令：

    # pcs resource move p_openstack-ceilometer-alarm-evaluator

`p_openstack-ceilometer-alarm-evaluator` 资源迁移的目标节点服务器由高可用集群自行决定。

如果要指定迁移的目标节点服务器（如：当前运行节点为 node-1 ，要迁移到 node-2 ），使用命令：

    # pcs resource move p_openstack-ceilometer-alarm-evaluator node-2

# p\_neutron-openvswitch-agent
`p_neutron-openvswitch-agent` 用于管理 neutron-openvswitch-agent 服务。由于 neutron-openvswitch-agent 可以同时启动多个实例，因此，在 `p_neutron-openvswitch-agent` 基础之上实现 `clone_p_neutron-openvswitch-agent` ,即所有 HA 集群节点服务器都会启动 1 个 `p_neutron-openvswitch-agent` 实例。

`p_neutron-openvswitch-agent`/`clone_p_neutron-openvswitch-agent` 不依赖高可用集群中的其它资源。

但是有其它的 2 个高可用集群资源依赖于 `clone_p_neutron-openvswitch-agent`，即：
1. `p_neutron-dhcp-agent`
1. `clone_p_neutron-l3-agent`

这 2 个资源在 `clone_p_neutron-openvswitch-agent` 启动之后才能启动，如果 `clone_p_neutron-openvswitch-agent` 被停止或禁用，这 2 个资源也将无法启动。

## p\_neutron-openvswitch-agent 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_neutron-openvswitch-agent` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_neutron-openvswitch-agent` 实例。

## 启动/停止 p\_neutron-openvswitch-agent
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_neutron-openvswitch-agent` 资源，由于 `clone_p_neutron-openvswitch-agent` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_neutron-openvswitch-agent

上述命令执行之后，`clone_p_neutron-openvswitch-agent` 就被禁用了，此时所有节点服务服务上都无法运行 `p_neutron-openvswitch-agent` 实例。同时依赖于 `clone_p_neutron-openvswitch-agent` 的 `p_neutron-dhcp-agent` 以及 `clone_p_neutron-l3-agent` 也将无法启动（已经启动的实例会被停止）。

要恢复被禁用的 `clone_p_neutron-openvswitch-agent` 资源，使用命令：

    # pcs resource enable clone_p_neutron-openvswitch-agent

## 临时停止某一节点上的 p\_neutron-openvswitch-agent 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_neutron-openvswitch-agent` 实例停用，使用命令：

    # pcs resource ban p_neutron-openvswitch-agent node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_neutron-openvswitch-agent` 实例了。同时由于`p_neutron-dhcp_agent` 和 `clone_p_neutron-l3-agent` 依赖于 `clone_p_neutron-openvswitch-agent` ，所以该节点上无法启动 `p_neutron-dhcp-agent` 和 `p_neutron-l3-agent` 实例（如果有已经启动的实例，会被停止）。

之后，如果要恢复在 node-2 节点上启动 `p_neutron-openvswitch-agent` 实例，则使用命令：

    # pcs resource clear p_neutron-openvswitch-agent node-2

# p\_neutron-dhcp-agent
`p_neutron-dhcp-agent` 用于管理 neutron-dhcp-agent 服务。。在高可用集群中属于一个普通资源，整个集群中只运行一个实例（即同一时刻只在集群之中的一台节点服务上运行）。

`p_neutron-dhcp-agent` 依赖高可用集群中的 `clone_p_neutron-openvswitch-agent`。

## p\_neutron-dhcp-agent 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

如果要确认 `p_neutron-dhcp-agent` 当前的运行节点，可以使用下面的命令：

    # crm_resource --locate --resource p_neutron-dhcp-agent

## 启动/停止 p\_neutron-dhcp-agent
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_neutron-dhcp-agent` 资源，使用命令：

    # pcs resource disable p_neutron-dhcp-agent

上述命令执行之后，`p_neutron-dhcp-agent` 就被禁用了，无法在任何一台节点服务器上启动。

要恢复被禁用的 `p_neutron-dhcp-agent` 资源，使用命令：

    # pcs resource enable p_neutron-dhcp-agent

## 迁移 p\_neutron-dhcp-agent 资源
要将 `p_neutron-dhcp-agent` 资源迁移到另一台节点服务器，使用命令：

    # pcs resource move p_neutron-dhcp-agent

`p_neutron-dhcp-agent` 资源迁移的目标节点服务器由高可用集群自行决定。

如果要指定迁移的目标节点服务器（如：当前运行节点为 node-1 ，要迁移到 node-2 ），使用命令：

    # pcs resource move p_neutron-dhcp-agent node-2

# p\_neutron-metadata-agent
`p_neutron-metadata-agent` 用于管理 neutron-metadata-agent 服务。由于 heat-engine 可以同时启动多个实例，因此，在 `p_neutron-metadata-agent` 基础之上实现 `clone_p_neutron-metadata-agent` ,即所有 HA 集群节点服务器都会启动 1 个 `p_neutron-metadata-agent` 实例。

`p_neutron-metadata-agent`/`clone_p_neutron-metadata-agent` 不依赖高可用集群中的其它资源。

## p\_neutron-metadata-agent 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_neutron-metadata-agent` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_neutron-metadata-agent` 实例。

## 启动/停止 p\_neutron-metadata-agent
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_neutron-metadata-agent` 资源，由于 `clone_p_neutron-metadata-agent` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_neutron-metadata-agent

上述命令执行之后，`clone_p_neutron-metadata-agent` 就被禁用了，此时所有节点服务服务上都无法运行 `p_neutron-metadata-agent` 实例。

要恢复被禁用的 `clone_p_neutron-metadata-agent` 资源，使用命令：

    # pcs resource enable clone_p_neutron-metadata-agent

## 临时停止某一节点上的 p\_neutron-metadata-agent 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_neutron-metadata-agent` 实例停用，使用命令：

    # pcs resource ban p_neutron-metadata-agent node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_neutron-metadata-agent` 实例了。

之后，如果要恢复在 node-2 节点上启动 `p_neutron-metadata-agent` 实例，则使用命令：

    # pcs resource clear p_neutron-metadata-agent node-2

# p\_neutron-l3-agent
`p_neutron-l3-agent` 用于管理 neutron-l3-agent 服务。由于 heat-engine 可以同时启动多个实例，因此，在 `p_neutron-l3-agent` 基础之上实现 `clone_p_neutron-l3-agent` ,即所有 HA 集群节点服务器都会启动 1 个 `p_neutron-l3-agent` 实例。

`p_neutron-l3-agent`/`clone_p_neutron-l3-agent` 依赖高可用集群中的 `clone_p_neutron-openvswitch-agent`。

## p\_neutron-l3-agent 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_neutron-l3-agent` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_neutron-l3-agent` 实例。

## 启动/停止 p\_neutron-l3-agent
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_neutron-l3-agent` 资源，由于 `clone_p_neutron-l3-agent` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_neutron-l3-agent

上述命令执行之后，`clone_p_neutron-l3-agent` 就被禁用了，此时所有节点服务服务上都无法运行 `p_neutron-l3-agent` 实例。

要恢复被禁用的 `clone_p_neutron-l3-agent` 资源，使用命令：

    # pcs resource enable clone_p_neutron-l3-agent

## 临时停止某一节点上的 p\_neutron-l3-agent 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_neutron-l3-agent` 实例停用，使用命令：

    # pcs resource ban p_neutron-l3-agent node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_neutron-l3-agent` 实例了。

之后，如果要恢复在 node-2 节点上启动 `p_neutron-l3-agent` 实例，则使用命令：

    # pcs resource clear p_neutron-l3-agent node-2

# p\_mysql
`p_mysql` 用于管理 MySQL 数据库服务。由于 MySQL 数据库实现了 Galera 集群，可以同时启动多个实例，因此，在 `p_mysql` 基础之上实现 `clone_p_mysql` ,即所有 HA 集群节点服务器都会启动 1 个 `p_mysql` 实例。

`p_mysql`/`clone_p_mysql` 不依赖高可用集群中的其它资源。

## p\_mysql 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_mysql` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_mysql` 实例。

## 启动/停止 p\_mysql
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_mysql` 资源，由于 `clone_p_mysql` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_mysql

上述命令执行之后，`clone_p_mysql` 就被禁用了，此时所有节点服务服务上都无法运行 `p_mysql` 实例。

要恢复被禁用的 `clone_p_mysql` 资源，使用命令：

    # pcs resource enable clone_p_mysql

## 临时停止某一节点上的 p\_mysql 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_mysql` 实例停用，使用命令：

    # pcs resource ban p_mysql node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_mysql` 实例了。

之后，如果要恢复在 node-2 节点上启动 `p_mysql` 实例，则使用命令：

    # pcs resource clear p_mysql node-2

# p\_rabbitmq-server
`p_rabbitmq-server` 用于管理 RabbitMQ 消息队列服务。由于 RabbitMQ 消息队列服务使用 cluster 和 HA queues，可以同时启动多个实例，因此，在 `p_rabbitmq-server` 基础之上实现 `clone_p_rabbitmq-server` ,即所有 HA 集群节点服务器都会启动 1 个 `p_rabbitmq-server` 实例。

`p_rabbitmq-server`/`clone_p_rabbitmq-server` 不依赖高可用集群中的其它资源。

## p\_rabbitmq-server 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_rabbitmq-server` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_rabbitmq-server` 实例。

## 启动/停止 p\_rabbitmq-server
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_rabbitmq-server` 资源，由于 `clone_p_rabbitmq-server` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_rabbitmq-server

上述命令执行之后，`clone_p_rabbitmq-server` 就被禁用了，此时所有节点服务服务上都无法运行 `p_rabbitmq-server` 实例。

要恢复被禁用的 `clone_p_rabbitmq-server` 资源，使用命令：

    # pcs resource enable clone_p_rabbitmq-server

## 临时停止某一节点上的 p\_rabbitmq-server 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_rabbitmq-server` 实例停用，使用命令：

    # pcs resource ban p_rabbitmq-server node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_rabbitmq-server` 实例了。

之后，如果要恢复在 node-2 节点上启动 `p_rabbitmq-server` 实例，则使用命令：

    # pcs resource clear p_rabbitmq-server node-2

# p\_haproxy
`p_haproxy` 用于管理 neutron-openvswitch-agent 服务。由于 neutron-openvswitch-agent 可以同时启动多个实例，因此，在 `p_haproxy` 基础之上实现 `clone_p_haproxy` ,即所有 HA 集群节点服务器都会启动 1 个 `p_haproxy` 实例。

`p_haproxy`/`clone_p_haproxy` 不依赖高可用集群中的其它资源。

但是有其它的 2 个高可用集群资源依赖于 `clone_p_haproxy`，即：
1. `vip__public`
1. `vip__management`

这 2 个资源在 `clone_p_haproxy` 启动之后才能启动，如果 `clone_p_haproxy` 被停止或禁用，这 2 个资源也将无法启动。

## p\_haproxy 运行状态
可以通过

    # pcs resource

查看所有资源的运行状态。

由于 `clone_p_haproxy` 是 clone 资源，所以在 `pcs resource` 中可以看到当前哪些节点服务上运行了 `p_haproxy` 实例。

## 启动/停止 p\_haproxy
在高可用集群中资源的启动和停止都由集群和资源配置控制。

默认情况下，高可用集群中的资源都是会启动（只要该资源启动所需的条件都满足），因此不需要手动启动资源。

要停止 `p_haproxy` 资源，由于 `clone_p_haproxy` 是 clone 资源，所以可以使用命令：

    # pcs resource disable clone_p_haproxy

上述命令执行之后，`clone_p_haproxy` 就被禁用了，此时所有节点服务服务上都无法运行 `p_haproxy` 实例。同时依赖于 `clone_p_haproxy` 的 `vip__public` 以及 `vip__management` 也将无法启动（已经启动的实例会被停止）。

要恢复被禁用的 `clone_p_haproxy` 资源，使用命令：

    # pcs resource enable clone_p_haproxy

## 临时停止某一节点上的 p\_haproxy 实例
默认情况下，clone 资源在所有节点上都会运行一个实例，但是如果要临时在某一节点服务器上停用该资源的实例，也是可以的。

如，要将 node-2 节点服务器的 `p_haproxy` 实例停用，使用命令：

    # pcs resource ban p_haproxy node-2

上述命令执行成功之后，在 node-2 节点上就不会再启动 `p_haproxy` 实例了。同时由于`vip__public` 和 `vip__management` 依赖于 `clone_p_haproxy` ，所以该节点上无法启动 `vip__public` 和 `vip__management` 实例（如果有已经启动的实例，会被停止）。

之后，如果要恢复在 node-2 节点上启动 `p_haproxy` 实例，则使用命令：

    # pcs resource clear p_haproxy node-2
