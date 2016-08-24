# EayunStack网络检测

该命令用于对EayunStack环境中的网络状态进行检测，现在只包含对vrouter的检测。

## 命令格式

```
(controller)# eayunstack doctor net -h  
usage: eayunstack doctor net [-h] COMMAND ...

Check openstack network. e.g. virtual router

optional arguments:
  -h, --help  show this help message and exit

Commands:
  COMMAND     DESCRIPTION
    vrouter   Check Neutron virtual router
```
```
(controller)# eayunstack  doctor net vrouter -h
usage: eayunstack doctor net vrouter [-h] [--tid TID] [--pid PID] [--rid RID]

Check Neutron virtual router

optional arguments:
  -h, --help  show this help message and exit
  --tid TID   Tenant ID
  --pid PID   Port ID
  --rid RID   Router ID

```

>###注意
>
>该命令只在Controller节点上运行。

## vrouter检测

* Controller节点执行命令(所有vrouter相关的检测)

```
(controller)# eayunstack doctor net vrouter
[ INFO  ] (controller) (node-15.eayun.com): start checking route wx-route[0f9ff230-b3d6-4382-9e5f-58a388f5818d]
[ ERROR ] (controller) (node-15.eayun.com): qos was not found on external interface qg-edb5aafb-43 of qrouter-0f9ff230-b3d6-4382-9e5f-58a388f5818d on node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): finish checking route wx-route[0f9ff230-b3d6-4382-9e5f-58a388f5818d]
[ INFO  ] (controller) (node-15.eayun.com): start checking route eayun2-router[10666a45-ec39-44f3-ac67-546148b9cce2]
[ ERROR ] (controller) (node-15.eayun.com): qos was not found on external interface qg-50540349-4f of qrouter-10666a45-ec39-44f3-ac67-546148b9cce2 on node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): finish checking route eayun2-router[10666a45-ec39-44f3-ac67-546148b9cce2]
[ INFO  ] (controller) (node-15.eayun.com): start checking route test_route[10f0af93-8a87-4573-8c8c-58f590ce5e42]
[ ERROR ] (controller) (node-15.eayun.com): qos was not found on external interface qg-3b460f9b-f9 of qrouter-10f0af93-8a87-4573-8c8c-58f590ce5e42 on node-15.eayun.com
[ INFO  ] (controller) (node-15.eayun.com): finish checking route test_route[10f0af93-8a87-4573-8c8c-58f590ce5e42]
[ INFO  ] (controller) (node-15.eayun.com): start checking route rally_router[169e460d-16ff-4c25-bd2a-47250772541c]

```

* Controller节点执行命令（检测某个租户）

```
(controller)# eayunstack --debug doctor net vrouter --tid 6be3afcf3fb94a5581288b36c95a38bc
[ INFO  ] (controller) (node-15.eayun.com): start checking route 彭大可[4230598e-30ec-484a-8bc2-a9d0c1bda085]
[ DEBUG ] (controller) (node-15.eayun.com): start checking port [32c2f697-ab8d-4448-83c6-445792c364a8]
[ DEBUG ] (controller) (node-15.eayun.com): check gateway for port on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): this port is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.4.1 on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ DEBUG ] (controller) (node-15.eayun.com): check external interface qg-32c2f697-ab qos on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): qos was found on external interface qg-32c2f697-ab of qrouter-4230598e-30ec-484a-8bc2-a9d0c1bda085 on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port [32c2f697-ab8d-4448-83c6-445792c364a8]
[ INFO  ] (controller) (node-15.eayun.com): finish checking route 彭大可[4230598e-30ec-484a-8bc2-a9d0c1bda085]

```

* Controller节点执行命令（检测某个端口）

```
(controller)# eayunstack --debug doctor net vrouter --pid 32c2f697-ab8d-4448-83c6-445792c364a8
[ DEBUG ] (controller) (node-15.eayun.com): check gateway for port on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): this port is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.4.1 on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ DEBUG ] (controller) (node-15.eayun.com): check external interface qg-32c2f697-ab qos on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): qos was found on external interface qg-32c2f697-ab of qrouter-4230598e-30ec-484a-8bc2-a9d0c1bda085 on node-15.eayun.com

```

* Controller节点执行命令（检测某个路由）

```
(controller)# eayunstack --debug doctor net vrouter --rid 4230598e-30ec-484a-8bc2-a9d0c1bda085
[ DEBUG ] (controller) (node-15.eayun.com): start checking port [32c2f697-ab8d-4448-83c6-445792c364a8]
[ DEBUG ] (controller) (node-15.eayun.com): check gateway for port on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): this port is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.4.1 on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ DEBUG ] (controller) (node-15.eayun.com): check external interface qg-32c2f697-ab qos on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): qos was found on external interface qg-32c2f697-ab of qrouter-4230598e-30ec-484a-8bc2-a9d0c1bda085 on node-15.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port [32c2f697-ab8d-4448-83c6-445792c364a8]

```

