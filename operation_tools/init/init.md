# 初始化命令

该命令用于对EayunStack环境进行初始化操作，在整个环境中的所有节点生成保存了节点角色和节点信息列表的配置文件；升级环境中所有节点上的 eayunstack-tools。

> **注意**
>
> 在EayunStack环境部署成功后，**必须**要执行该初始化命令对环境进行初始化！

## 命令格式

> ###### 注意
>
> 该命令只可在**Fuel节点**执行

```
(fuel)# eayunstack init --help
usage: eayunstack init [-h] [-u]

EayunStack Environment Initialization

optional arguments:
  -h, --help    show this help message and exit
  -u, --update  Update this tool on all nodes
```

## 初始化EayunStack环境

```
[fuel]$ eayunstack init
Please enter environment name []: EayunStack_Test_Env
[ INFO  ] Generate node-list file ...
[ INFO  ] Copy node-list file to all nodes ...
[ INFO  ]    To node 172.16.100.9 ...
[ INFO  ]    To node 172.16.100.13 ...
[ INFO  ]    To node 172.16.100.5 ...
[ INFO  ]    To node 172.16.100.8 ...
[ INFO  ]    To node 172.16.100.11 ...
[ INFO  ]    To node 172.16.100.6 ...
[ INFO  ]    To node 172.16.100.12 ...
[ INFO  ]    To node 172.16.100.14 ...
[ INFO  ]    To node 172.16.100.7 ...
[ INFO  ] Generate node-role file for fuel node ...
[ INFO  ] Generate node-role file for openstack node ...
[ INFO  ]    To node 172.16.100.9 ...
[ INFO  ]    To node 172.16.100.13 ...
[ INFO  ]    To node 172.16.100.5 ...
[ INFO  ]    To node 172.16.100.8 ...
[ INFO  ]    To node 172.16.100.11 ...
[ INFO  ]    To node 172.16.100.6 ...
[ INFO  ]    To node 172.16.100.12 ...
[ INFO  ]    To node 172.16.100.14 ...
[ INFO  ]    To node 172.16.100.7 ...
```

## 升级所有节点上的 eayunstack-tools

> 注意
>
> 升级前需要先同步 EayunStack 最新升级源。

```
(fuel)# eayunstack init -u
[ INFO  ] (fuel) (fuel.domain.tld): Update on node-20.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-21.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-16.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-24.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-14.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-17.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-18.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-15.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

[ INFO  ] (fuel) (fuel.domain.tld): Update on node-22.eayun.com successfully.
[ INFO  ] (fuel) (fuel.domain.tld): Current version: 1.1-8

```