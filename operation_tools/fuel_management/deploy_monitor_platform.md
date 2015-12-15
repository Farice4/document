# 部署监控平台

监控平台部署分为两部分：influxdb/grafana 服务部署及 lma_collector 服务部署。

>  **注意**
>
>  influxdb/grafana 服务需要部署在一台安装了 CentOS 7 系统的独立的服务器（即以下示例中的“influxdb node”）上，关于该服务器的部署方法，请参考《EayunStack部署手册》v1.1 监控平台部署章节。

## 命令格式

```
[fuel]# eayunstack fuel deployment_monitor_plugins --help
usage: eayunstack fuel deployment_monitor_plugins [-h] --env ENV [--influxdb]
                                                  [--lma_collector]

Deploy fuel monitor plugins.

optional arguments:
  -h, --help       show this help message and exit
  --env ENV        The fuel environment to be deployment.
  --influxdb       Deployment influxdb node.
  --lma_collector  Deployment lma_collector on openstack node.
```

## 部署 influxdb/grafana 服务

influxdb 数据库服务用于存放所有监控数据，grafana 为监控平台的 WEB UI。

* 登陆 Fuel 节点，确认要部署监控平台的环境的环境 ID

```
[root@fuel ~](fuel)# fuel node list
id | status | name             | cluster | ip         | mac               | roles      | pending_roles | online | group_id
---|--------|------------------|---------|------------|-------------------|------------|---------------|--------|---------
8  | ready  | Untitled (db:a1) | 1       | 10.20.0.13 | 26:6f:ac:f7:cf:4d | mongo      |               | True   | 1
2  | ready  | Untitled (00:30) | 1       | 10.20.0.12 | f2:42:eb:5d:71:42 | controller |               | True   | 1
7  | ready  | Untitled (35:d7) | 1       | 10.20.0.3  | 72:80:a7:6d:8e:4b | ceph-osd   |               | True   | 1
5  | ready  | Untitled (b2:4b) | 1       | 10.20.0.5  | ee:32:5a:7b:f3:4d | ceph-osd   |               | True   | 1
4  | ready  | Untitled (42:3e) | 1       | 10.20.0.8  | 0e:d7:90:ee:c0:43 | compute    |               | True   | 1
1  | ready  | Untitled (3c:2a) | 1       | 10.20.0.10 | 9e:a9:62:d2:8f:40 | controller |               | True   | 1
6  | ready  | Untitled (70:0b) | 1       | 10.20.0.11 | 52:fd:fe:ab:bb:4a | ceph-osd   |               | True   | 1
3  | ready  | Untitled (cf:d3) | 1       | 10.20.0.7  | ae:32:74:02:3b:47 | controller |               | True   | 1
```

通过以上信息可以确认环境 ID 为 "1"（ cluster 列所标示 ID）

* 执行命令，部署 influxdb/grafana 服务

```
[root@fuel ~](fuel)# eayunstack fuel deployment_monitor_plugins --env 1 --influxdb
Please entry the ip address of influxdb node: 10.20.0.250
Please entry the root\'s password of influxdb node: 
[ INFO  ] (fuel) (fuel.domain.tld): Generate conf file .
Please entry influxdb_dbname [lma]: lma
Please entry influxdb_rootpass [admin]: admin
Please entry influxdb_username [lma]: lma
Please entry influxdb_userpass [lmapass]: lmapass
[ INFO  ] (fuel) (fuel.domain.tld): Push conf file to influxdb node.
[ INFO  ] (fuel) (fuel.domain.tld): Install rpm packages "puppet rsync" on node 10.20.0.250.
[ INFO  ] (fuel) (fuel.domain.tld): Deploy influxdb/grafana on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest check_environment_configuration.pp on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest eayunstack_netconfig.pp on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest firewall.pp on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest setup_influxdir.pp on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest influxdb.pp on node 10.20.0.250 .
[ INFO  ] (fuel) (fuel.domain.tld): Apply manifest grafana.pp on node 10.20.0.250 .
```

>  **提示**
>
>  部署成功后，可以通过浏览器访问监控平台服务器外网 IP 地址（如：25.0.2.254）访问监控平台 WEB UI。

## 部署 lma_collector 服务

lma_collector 服务运行在所有 OpenStack 节点，用于实时收集节点信息，并将信息发送到监控节点。

* 执行命令，部署 lma_collector 服务

```
[root@fuel ~](fuel)# eayunstack fuel deployment_monitor_plugins --env 1 --lma_collector
[ INFO  ] (fuel) (fuel.domain.tld): Checking all openstack node is online ...
[ INFO  ] (fuel) (fuel.domain.tld): Generate openstack node conf file.
[ INFO  ] (fuel) (fuel.domain.tld): Push conf file to openstack node.
[ INFO  ] (fuel) (fuel.domain.tld): Create symbolic links on openstack node.
[ INFO  ] (fuel) (fuel.domain.tld): Rsync plugin modules on openstack node.
[ INFO  ] (fuel) (fuel.domain.tld): Deployment lma_collector on openstack nodes.
```

