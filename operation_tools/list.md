# 列表显示节点信息命令

该命令用于列出EayunStack环境中所有节点信息。

## 命令格式

> ###### 注意
>
> 该命令可以在**任意节点**执行

```
# eayunstack list --help
usage: eayunstack list [-h]

List OpenStack Node

optional arguments:
  -h, --help  show this help message and exit
```

## 列出环境中所有节点信息

```
# eayunstack list
+------------+-------------------+---------------+-------------------+---------------+
|   Roles    |       Hosts       |       IP      |        MAC        |   IDRac_Addr  |
+------------+-------------------+---------------+-------------------+---------------+
|  ceph-osd  |  node-1.eayun.com |  172.16.100.3 | 8e:51:b0:02:fd:46 | 192.168.1.235 |
|  ceph-osd  |  node-2.eayun.com |  172.16.100.4 | 4a:9e:7b:80:22:4e | 192.168.1.236 |
|  ceph-osd  |  node-3.eayun.com |  172.16.100.5 | 3e:0f:52:9d:b3:44 | 192.168.1.237 |
|  compute   | node-10.eayun.com | 172.16.100.12 | 8a:03:a4:88:c2:41 | 192.168.1.244 |
|  compute   |  node-4.eayun.com |  172.16.100.7 | 5e:2c:22:51:f4:45 | 192.168.1.241 |
|  compute   |  node-7.eayun.com |  172.16.100.9 | 26:44:40:e9:ef:46 | 192.168.1.243 |
| controller |  node-5.eayun.com |  172.16.100.6 | 6a:88:95:76:9e:46 | 192.168.1.239 |
| controller |  node-6.eayun.com |  172.16.100.8 | 52:a8:3b:dc:c9:47 | 192.168.1.238 |
| controller |  node-8.eayun.com | 172.16.100.10 | e2:94:d6:1c:80:4e | 192.168.1.240 |
|   mongo    |  node-9.eayun.com | 172.16.100.11 | 2a:85:6a:29:77:4b | 192.168.1.242 |
+------------+-------------------+---------------+-------------------+---------------+
```
