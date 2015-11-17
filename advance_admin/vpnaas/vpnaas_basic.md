# VPNaaS基本知识即部署

# VPNaaS的基本知识

1.
什么是VPNaaS:即VPN服务

2.
VPNaaS中的几个重要元素：

a.ike policy:ike 全称是 Internet Key Exchange，是 IPSec 协议族之下的一种网络协议。Neutron 中的 VPNaaS 服务使用 ike 来进行密钥的管理，就是通过 ike policy 来完成的。

b.ipsec policy：用以配置 Neutron 中 VPNaaS 的 IPSec 参数，包括 ipsec 的加密算法，使用模式、密钥生存期以及使用协议等等

c.vpn service：表示一个 VPNaaS 的服务，应用在需要建立VPN的主机上

d.ipsec site connection：表示连接到远端的一个 ipsec 站点连接

备注：此处的ike policy和ipsec policy都相当于只是为VPN选择加密方式，而neutron中的VPNaaS目前只支持IPSec协议族下的协议。


# VPNaaS的部署
由于 VPNaaS 的部署过程比较复杂且容易出错，因此已将其简化为一个 fuel 插件来完成相关的工作，只需要在 fuel 的设置界面上勾选上使用 Neutron 的 VPNaaS 功能即可。


