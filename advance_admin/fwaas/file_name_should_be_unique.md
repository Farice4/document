# FWaaS基本知识
什么是FWaaS：即防火墙服务。openstack上的防火墙服务基本上是基于linux的iptables规则而来。

FWaaS有三个基本元素：firewall-rule,firewall-policy,firewall。
firewall:对应防火墙，包含并只包含一个 firewall-policy，创建时即需要指定对应的 firewall-policy。

firewall-policy：对应防火墙策略，包含多个 firewall-rule，规则有先后顺序。

firewall-rule：对应具体的防火墙规则，五元组匹配网络流量加上对应执行的动作。

一个环境中只能有一个firewall，一个firewall只能有一个firewall-policy，一个firew
all-policy可以有多个firewall-rule

# FWaaS工作原理
(1)、防火墙发生的地点：发生在路由连接内网的端口，每个路由器存在network节点中，在每个路由上有自己独立的net namespace隔离，名字通常是qrouter-路由ID

(2)、路由有两个简单的端口分别是qg和qr，其中qg对应着gateway连接着外网，qr对应着router连接着内网，名字通常是qr/qg-port-id

(3)、防火墙的功能在namespace里的filter表里，filter表用作过滤。用namespace隔离着。所以可以通过操作namespace来操作防火墙。

# FWaaS的部署
1.在network节点上

a.修改neutron-l3-agent的配置文件`/etc/neutron/neutron.conf`
>```
[fwaas]
driver=neutron.services.firewall.drivers.linux.iptables_fwaas.IptablesFwaasDriver
enabled = True
```

b.重启neutron-l3-agent服务，执行命令：
>```
systemctl restart neutron-l3-agent.service
```

2.在controller节点上

a.修改配置文件`/etc/neutron/neutron.conf` ，修改[DEFAULT]小节的service_plugins参数：
```
service_plugins = ...,neutron.services.firewall.fwaas_plugin.FirewallPlugin
```
b.重启neutron-server服务

c.修改horizon的local_setting，将enable_firewall设置为Ture：
```
OPENSTACK_NEUTRON_NETWORK = {
    ...
    'enable_firewall': True,
    ...
}
```
d.重启horizon，即重启httpd服务


# FWaaS的高可用部署

FWaaS 在几个插件之中相对来说高可用是最简单的，因为它其实是用 l3-agent 实现的，而 l3-agent 的高可用在 fuel 部署完成的节点上已经配置完成了。


1、在 network 节点上

 a.由于为 pacemaker 提供的脚本中启动 neutron-l3-agent 的参数与 systemd 服务文件中的启动参数不同，没有加上 `–config-file=/etc/neutron/fwaas_driver.ini`，所以上面那一节参数的修改要放在 `/etc/neutron/l3_agent.ini` 中

 b.重启 neutron-l3-agent 服务的方式不同了，必须使用 pacemaker，运行：`pcs resource disable p_neutron-l3-agent` 确认该服务资源在所有节点停止之后再执行 `pcs resource enable p_neutron-l3-agent6)`

2、在 controller 节点上操作同上面简单的部署一样















