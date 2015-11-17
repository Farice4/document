# pcs 命令 #
在 EL7 之后的系统中，基于 Corosync + Pacemaker 的集群，主要的管理命令是 pcs 。

    # pcs help

    Usage: pcs [-f file] [-h] [commands]...
    Control and configure pacemaker and corosync.

    Options:
        -h, --help  Display usage and exit
        -f file     Perform actions on file instead of active CIB
        --debug     Print all network traffic and external commands run
        --version   Print pcs version information

    Commands:
        cluster     Configure cluster options and nodes
        resource    Manage cluster resources
        stonith     Configure fence devices
        constraint  Set resource constraints
        property    Set pacemaker properties
        status      View cluster status
        config      Print full cluster configuration

而 pcs 的这些子命令的使用方法同样可以通过：

    # pcs [command] help

查看。

# 查看集群状态信息 #

    # pcs status [commands]

该命令不加后续子命令时，显示集群状态大部分的状态信息，即包括了 resources 、groups 、 cluster 、corosync 、nodes 以及 pcsd ；加上子命令时，可以只查看相应的一部分，如：

    # pcs status nodes
    Pacemaker Nodes:
     Online: node-1.domain.tld node-2.domain.tld node-3.domain.tld
     Standby:
     Offline:


# 查看当前整个集群的配置 #

    # pcs config


# 启动/停止 Corosync 和 Pacemaker 服务 #
启动和停止 Corosync 、 Pacemaker 服务，可以使用系统服务管理命令，如 systemctl 。

也可以使用 pcs cluster ：

    启动 Corosync + Pacemaker
    # pcs cluster start [--all]

    停止 Corosync + Pacemaker
    # pcs cluster stop [--all]

如果加上 **--all** 参数，则是对 HA 集群中所有的节点进行操作。

# 临时停用/重新启用某一节点 #

    临时停用某一节点：
    # pcs cluster standby [node-name]
    重新启用之前停用的节点：
    # pcs cluster unstandby [node-name]

一旦节点停用，该节点上之前运行的所有资源将根据集群策略以及当前状态进行迁移或者停止。

# 查看集群资源 #
## 资源列表及状态 ##

    # pcs resource
    或者
    # pcs resource show


## 资源配置信息 ##
查看所有资源的配置信息

    # pcs resource show [resource-id]


只查看某一资源的配置信息：

    # pcs resource show [resource-id]

如：

    # pcs resource show vip__public
     Resource: vip__public (class=ocf provider=mirantis type=ns_IPaddr2)
      Attributes: nic=br-ex base_veth=br-ex-hapr ns_veth=hapr-p ip=25.0.0.2 iflabel=ka cidr_netmask=24 ns=haproxy gateway=link gateway_metric=10 other_networks=false iptables_start_rules="iptables -t mangle -I PREROUTING -i br-ex-hapr -j MARK --set-mark 0x2a ; iptables -t nat -I POSTROUTING -m mark --mark 0x2a ! -o br-ex -j MASQUERADE" iptables_stop_rules="iptables -t mangle -D PREROUTING -i br-ex-hapr -j MARK --set-mark 0x2a ; iptables -t nat -D POSTROUTING -m mark --mark 0x2a ! -o br-ex -j MASQUERADE" iptables_comment=masquerade-for-public-net
      Meta Attrs: migration-threshold=3 failure-timeout=60 resource-stickiness=1
      Operations: monitor interval=3 timeout=30 (vip__public-monitor-3)
                  start interval=0 timeout=30 (vip__public-start-0)
                  stop interval=0 timeout=30 (vip__public-stop-0)


# 启用/禁用集群资源 #
启用集群资源

    # pcs resource enable <resource-id>


禁用集群资源：

    # pcs resource disable <resource-id>

一旦禁用某一资源，则该资源不能在任何节点启动。如果该资源已经启动，首先会进行停止操作。

# 临时禁止在某一节点启动某资源 #
## 临时禁止在当前运行某资源实例的节点上启动该资源： ##

    # pcs resource ban <resource-id>


## 临时禁止在指定节点启动某资源： ##

    # pcs resource ban <resource-id> <node-name>


## 临时禁止在某节点上启动 stateful 资源的 Master 实例 ##
禁止当前运行某资源 Master 实例的节点上启动 stateful 资源的 Master 实例

    # pcs resource ban <stateful-resource-id> --master


禁止指定节点启动 stateful 资源的 Master 实例

    # pcs resource ban <stateful-resource-id> <node-name> --master


> #### 重要
> 提醒用户进一步操作之前必须谨慎
>stateful-resource-id 对应于基础资源之上的 stateful 资源，如:
>
    # pcs resource show master_p_rabbitmq-server
>     Master: master_p_rabbitmq-server
>      Meta Attrs: notify=true ordered=false interleave=true master-max=1 master-node-max=1 target-role=Started
>      Resource: p_rabbitmq-server (class=ocf provider=mirantis type=rabbitmq-server)
>       Attributes: node_port=5673 command_timeout="-s KILL"
>       Meta Attrs: migration-threshold=INFINITY failure-timeout=180s
>       Operations: monitor interval=30 timeout=60 (p_rabbitmq-server-monitor-30)
>                   monitor interval=27 role=Master timeout=60 (p_rabbitmq-server-monitor-27)
>                   start interval=0 timeout=360 (p_rabbitmq-server-start-0)
>                   stop interval=0 timeout=60 (p_rabbitmq-server-stop-0)
>                   promote interval=0 timeout=120 (p_rabbitmq-server-promote-0)
>                   demote interval=0 timeout=60 (p_rabbitmq-server-demote-0)
>                   notify interval=0 timeout=180 (p_rabbitmq-server-notify-0)


其中， p\_rabbitmq-server 是基础资源， master\_p\_rabbitmq-server 是 stateful 资源。

> #### 重要
> 目前 EayunStack 上没有　stateful 资源


## 取消之前 ban 命令产生的资源启动限制 ##

    # pcs resource clear <resource-id> [node-name]

不加节点名，取消所有关于该资源的临时限制；

如果加上节点名，则仅仅取消对该节点的限制

如果要取消对 stateful 资源 Master 实例的临时限制，应使用：

    # pcs resource clear <stateful-resource-id> [node-name]


# 手动迁移资源 #
请首先阅读：[清除 move 命令产生的资源限制](#pcs_resource_clear)。

## 基本迁移命令 ##
请首先阅读：[清除 move 命令产生的资源限制](#pcs_resource_clear)。


    # pcs resource move <resource-id>

资源迁移到哪一台节点，由 HA 集群自行决定。

## 指定资源迁移的目标节点 ##
请首先阅读：[清除 move 命令产生的资源限制](#pcs_resource_clear)。


    # pcs resource move <resource-id> <node-name>

资源将移到指定的节点。

## <a name="pcs_resource_clear" style="text-decoration: none; color: inherit;"/>清除 move 命令产生的资源限制 ##
move 命令和 ban 命令相似，都通过在集群配置增加限制规则来实现的，因此一旦执行了 move 命令，资源将无法再次在当前节点启动，为了清除这一限制，需要执行命令：

    # pcs resource clear <resource-id>
