本章主要对运行在以下节点的 OpenStack 相关的服务的集群和 HA 管理进行说明

| 角色 | 说明 |
| ---- | ---- |
| 控制和网络节点 | 运行glance，keystone，mysql等基本组建和网络服务 |
| 计算节点 | 运行计算服务 |
| MongoDB 节点 | 为 Ceilmetor 服务运行 MongoDB 服务 |
| Ceph 存储节点 | 运行 Ceph OSD 服务 |
| Eqlx 存储节点 | 运行 Dell Eqxl 存储服务 |
| Fuel 节点 | 运行 EayunStack 部署服务 |

### 模块说明

OpenStack 的集群管理主要包括以下几个模块的管理:

#### keystone

keystone 运行在控制节点中，集群管理通过 systemctl 实现，服务名是 openstack-keystone.service

#### glance

glance 服务运行在控制节点中，有两个主要的服务:

* **glance-api** 接受客户端的所有命令, 分发并响应, 转到 glance-registry 完成具体的IO和数据库工作
* **glance-registry** 实现涉及到数据库的操作， V2 版本的 API 简化了流程, 所有的处理都在内部实现, 所以不需要此服务

上面两个服务的集群管理通过 systemctl 实现，服务名分别是 openstack-glance-api.service 和 openstack-glance-registry.service,

#### cinder

在 EayunStack 中，所有的 cinder 服务都在控制节中运行，所有服务的集群管理通过 systemctl 实现：

* **cinder-api** 接受客户端的命令, 分发到 cinder-volume 去处理，服务名是 openstack-cinder-api.service
* **cinder-scheduler** cinder-api 使用该服务分发具体的 cinder-volume 去管理 cinder volume，服务名是 openstack-cinder-scheduler.service
* **cinder-volume** cinder 真正的存储后端，处理创建/删除/快照 cinder volumn 等，服务名是 openstack-cinder-volume.service
* **cinder-backup** 备份服务，可以将 cinder 卷备份到其他支持的存储(e.g. ceph)，服务名是 openstack-cinder-backup.service

#### nova

包括运行在控制节点中的 api，vnc等服务和运行在计算节点中的计算服务：

* **nova-api** 接受客户端管理云主机的命令, 分发到其它组件去处理，服务名是 openstack-nova-api.service
* **nova-cert** 提供 X509 认证服务，只用于 EC2 的API认证，再使用 EC2 的接口的时候，需要 nova-cert 生成对应 tenant 的 cert.pem 和 pk.pem。服务名是 openstack-nova-cert.service。目前没有使用该服务，为了未来的扩展默认开启。
* **nova-conductor** nova-compute 利用该服务来访问数据库，这样计算节点就不需要连接数据库，从而提高了安全性。服务名是 openstack-nova-conductor.service
* **nova-consoleauth** 为 nova 其他组件提供认证服务，比如为 nova-novncproxy 的客户端提供身份认证服务。服务名是 openstack-nova-consoleauth.service
* **nova-novncproxy** 利用 novnc 提供云主机的 vnc 访问。服务名是 openstack-nova-novncproxy.service。
* **nova-objectstore** 提供用于注册 S3 接口的镜像服务，转换 S3 请求为镜像服务的请求。服务名是 openstack-nova-objectstore.service。目前没有使用该服务，为了未来的扩展默认开启。
* **nova-scheduler** 接受 nova-api 的请求，决定在哪台计算节点启动云主机。服务名是 openstack-nova-scheduler.service。
* **nova-compute** 调用后端的 hypervisor API（e.g. libvirt）进行云主机的管理操作。如创建/删除/查看内存使用等。服务名是 openstack-nova-compute.service。运行在计算节点

#### neutron
neutron 的所有服务都运行在控制节点,
* **neutron-qos-agent** neutron 的 qos 组件, 用 systemctl 管理, 服务名是 neutron-qos-agent.service
* **neutron-server** neutron 的服务端, 用 systemctl 管理, 服务名是 neutron-server.service
* **neutron-openvswitch-agent** neutron 的openvswitch 插件, 使用 pacemaker 管理, 资源ID是 p_neutron-openvswitch-agent
* ** neutron-dhcp-agent** 用来提供dhcp服务, 使用 pacemaker 管理, 资源ID是p_neutron-dhcp-agent
* ** neutron-metadata-agent** 转发云主机的 metadata 请求到 nova-api, 使用 pacemaker 管理, 资源ID是 p_neutron-metadata-agent
* ** neutron-l3-agent** neutron 的 l3 agent, 提供l3层的网络功能, 使用 pacemaker 管理, 资源ID是 p_neutron-l3-agent
* ** neutron-lbaas-agent** 负载均衡agent, 提供网络负载均衡, 使用 pacemaker 管理, 资源ID是 p_neutron-lbaas-agent

#### horzion

使用 Apache WSGI 框架提供 OpenStack 的 web 服务，所以 horzion 的管理即 Apache 的管理。服务名是 httpd.service
