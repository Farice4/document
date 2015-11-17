# 列出OpenStack节点信息

管理员可以在Fuel节点查看EayunStack环境节点列表。

## 命令格式

```
[fuel]$ eayunstack manage list -h
usage: eayunstack manage list [-h]

List OpenStack Node

optional arguments:
  -h, --help  show this help message and exit
```

## 查看EayunStack环境节点列表

```
[fuel]$ eayunstack manage list
+------------+-------------------+---------------+-------------------+
|   Roles    |       Hosts       |       IP      |        MAC        |
+------------+-------------------+---------------+-------------------+
|  ceph-osd  |  node-7.eayun.com |  172.16.100.3 | de:91:9f:cd:e6:43 |
|  ceph-osd  |  node-8.eayun.com | 172.16.100.12 | 16:eb:b7:ae:fd:43 |
|  ceph-osd  |  node-9.eayun.com |  172.16.100.4 | 3a:94:30:a8:f4:43 |
|  compute   |  node-4.eayun.com | 172.16.100.10 | ca:55:64:bb:95:44 |
|  compute   |  node-5.eayun.com | 172.16.100.11 | 1a:bf:96:75:1e:40 |
|  compute   |  node-6.eayun.com |  172.16.100.9 | ca:9f:b1:00:15:4b |
| controller |  node-1.eayun.com |  172.16.100.6 | 92:97:74:10:2b:49 |
| controller |  node-2.eayun.com |  172.16.100.7 | 9a:5b:c6:63:9f:49 |
| controller |  node-3.eayun.com |  172.16.100.8 | 76:18:75:37:c0:4c |
|   mongo    | node-10.eayun.com |  172.16.100.5 | ee:b2:30:ec:e3:47 |
+------------+-------------------+---------------+-------------------+
```
