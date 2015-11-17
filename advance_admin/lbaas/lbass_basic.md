# LBaaS基本知识以及部署
# LBaaS的基本知识
1.
什么是LBaaS:负载均衡服务，允许租户动态的在自己的网络创建一个负载均衡设备，可以说是分布式系统中比较基础的组建。openstack neutron通过高级服务扩展形式支持LBaaS，目前默认通过HAProxy软件来实现

2.
LBaaS之中的几个基本元素：pool,member,vip,healthmonitor

pool:即负载均衡的资源池

member:即存在于pool之中的资源池成员，一般为一些主机

vip:需要负载均衡的一个主机(可以是一个主机也可以是一地址向外部提供服务)

healthmonitor:作为监听器，随时监听member和pool之间的连接的畅通性(通常一般使用ICMP协议，通过ping，监听是否正常)

# LBaaS的部署
1.
在network节点上
a.修改`neutron-lbaas-agent`的配置文件`/etc/neutron/lbaas_agent.ini`，确认[DEFAULT]小节的配置如下：

```
[DEFAULT]
interface_driver = neutron.agent.linux.interface.OVSInterfaceDriver
ovs_use_veth = True
device_driver = neutron.services.loadbalancer.drivers.haproxy.namespace_driver.HaproxyNSDriver
```
b.启动/重启 neutron-lbaas-agent 服务

2.
在controller节点上
a.修改`/etc/neutron/neutron.conf`文件，修改[DEFAULT]小节的service_plugins参数的值，如下:

```
[DEFAULT]
service_plugins = ...,neutron.services.loadbalancer.plugin.LoadBalancerPlugin
```

b.修改`/etc/neutron/neutron.conf`文件，修改[service_providers]小节，确保其中有这样一行内容:

```
[service_providers]
service_provider = LOADBALANCER:Haproxy:neutron.services.loadbalancer.drivers.haproxy.plugin_driver.HaproxyOnHostPluginDriver:default
```

c.重启neutron-server服务

d.修改horizon的local_settings，将enable_lb设置为True:

```
OPENSTACK_NEUTRON_NETWORK = {
    ...
    'enable_lb': True,
    ...
}
```

e.重启horizon，即重启httpd服务




