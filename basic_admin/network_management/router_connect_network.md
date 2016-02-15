# 外部/内部网络与路由器之间的操作

### 通过Web horizon配置外部/内部网络与路由器之间操作

* 登录Web horizon点击项目----网络---路由---选择路由----设置网关

![Router_Connect](../Picture/router_connect1.jpg)

> 外部网络选择已建好的外部网络或者外部子网名称，用来提供外部网络访问云主机的网络

* 路由与外部网络配置完成

![Router_Connect](../Picture/router_connect2.jpg)

* 选择路由----点击新增接口---选择内部网络

![Router_Connect](../Picture/router_connect3.jpg)

> 当租户网络连接路由器时需要在路由器上点击新增接口,选择创建的租户网络

* 点击增加接口，路由与内部网络连接配置完成

![Router_Connect](../Picture/router_connect4.jpg)

* 点击网络拓扑标签，查看网络与路由连接情况，此时路由器与新建内部，外部网络连接

![Router_Connect](../Picture/router_connect5.jpg)

* 可通过新建云主机来测试新建网络

![Router_Connect](../Picture/router_connect6.jpg)


名称解析:
|名称|说明|
|----|----|
|网关|网络连接时内部网络向外进行数据交换的出口，这里连接的网关是外部网络中一个接口|
|接口|路由器上的一个端口，创建的路由器在与内部网络连接时，需要通过增加端口来连接内部网络|

备注:　创建网络时，外部网络默认不要选择自动分配，内部网络选择自动分配，创建云主机时，云主机
　　　将从内部网络dhcp获取到内部网络的IP地址．
