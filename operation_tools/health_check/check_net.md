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
[ INFO  ] (controller) (node-15.eayun.com): start checking route eayun2-router(10666a45-ec39-44f3-ac67-546148b9cce2)
[ INFO  ] (controller) (node-15.eayun.com): finish checking route eayun2-router(10666a45-ec39-44f3-ac67-546148b9cce2)
[ INFO  ] (controller) (node-15.eayun.com): start checking route rally_router(169e460d-16ff-4c25-bd2a-47250772541c)
[ INFO  ] (controller) (node-15.eayun.com): finish checking route rally_router(169e460d-16ff-4c25-bd2a-47250772541c)
[ INFO  ] (controller) (node-15.eayun.com): start checking route zk(17448d6a-6501-47aa-af37-b217c02d0e13)
[ INFO  ] (controller) (node-15.eayun.com): finish checking route zk(17448d6a-6501-47aa-af37-b217c02d0e13)
```

* Controller节点执行命令（检测某个租户）

```
(controller)# eayunstack --debug doctor net vrouter --tid 33df23b90b72492cb1c87e3cf2184fa3
[ INFO  ] (controller) (node-15.eayun.com): start checking route test_router(5e3d6344-2f60-4cca-beea-eec034dcab21)
[ DEBUG ] (controller) (node-15.eayun.com): start checking port (54fa9336-841b-473e-bbb0-ab96f2ffd1e9)
[ DEBUG ] (controller) (node-15.eayun.com): status of port 54fa9336-841b-473e-bbb0-ab96f2ffd1e9(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): admin_status of port 54fa9336-841b-473e-bbb0-ab96f2ffd1e9(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): this port(54fa9336-841b-473e-bbb0-ab96f2ffd1e9) is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.0.1 on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ DEBUG ] (controller) (node-15.eayun.com): check external interface qg-54fa9336-84 qos on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): qos was found on external interface qg-54fa9336-84 of qrouter-5e3d6344-2f60-4cca-beea-eec034dcab21 on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port (54fa9336-841b-473e-bbb0-ab96f2ffd1e9)
[ DEBUG ] (controller) (node-15.eayun.com): start checking port (8261387e-7188-4042-9319-72e4b22b6801)
[ DEBUG ] (controller) (node-15.eayun.com): status of port 8261387e-7188-4042-9319-72e4b22b6801(network:router_interface) is up
[ DEBUG ] (controller) (node-15.eayun.com): admin_status of port 8261387e-7188-4042-9319-72e4b22b6801(network:router_interface) is up
[ DEBUG ] (controller) (node-15.eayun.com): this port(8261387e-7188-4042-9319-72e4b22b6801) is network:router_interface port, do not need to check gateway
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port (8261387e-7188-4042-9319-72e4b22b6801)
[ INFO  ] (controller) (node-15.eayun.com): finish checking route test_router(5e3d6344-2f60-4cca-beea-eec034dcab21)
```

* Controller节点执行命令（检测某个端口）

```
(controller)# eayunstack --debug doctor net vrouter --pid 32c2f697-ab8d-4448-83c6-445792c364a8
[ DEBUG ] (controller) (node-15.eayun.com): status of port 32c2f697-ab8d-4448-83c6-445792c364a8(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): admin_status of port 32c2f697-ab8d-4448-83c6-445792c364a8(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): this port(32c2f697-ab8d-4448-83c6-445792c364a8) is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.4.1 on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ INFO  ] (controller) (node-15.eayun.com): there is no subnet attach to router 4230598e-30ec-484a-8bc2-a9d0c1bda085,skip qos check.
```

* Controller节点执行命令（检测某个路由）

```
(controller)# eayunstack --debug doctor net vrouter --rid e0df8a0d-4856-4ce9-b4e3-a193ec6f6a46
[ DEBUG ] (controller) (node-15.eayun.com): start checking port (7d4edcf4-1e52-4dbd-b397-bb1250663447)
[ DEBUG ] (controller) (node-15.eayun.com): status of port 7d4edcf4-1e52-4dbd-b397-bb1250663447(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): admin_status of port 7d4edcf4-1e52-4dbd-b397-bb1250663447(network:router_gateway) is up
[ DEBUG ] (controller) (node-15.eayun.com): this port(7d4edcf4-1e52-4dbd-b397-bb1250663447) is external port, check external gateway
[ DEBUG ] (controller) (node-15.eayun.com): check external gateway 25.0.4.1 on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): external gateway is ok
[ DEBUG ] (controller) (node-15.eayun.com): check external interface qg-7d4edcf4-1e qos on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): qos was found on external interface qg-7d4edcf4-1e of qrouter-e0df8a0d-4856-4ce9-b4e3-a193ec6f6a46 on node-18.eayun.com
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port (7d4edcf4-1e52-4dbd-b397-bb1250663447)
[ DEBUG ] (controller) (node-15.eayun.com): start checking port (ed5e4596-e7c7-4dda-8b4e-fa9437407ecd)
[ DEBUG ] (controller) (node-15.eayun.com): status of port ed5e4596-e7c7-4dda-8b4e-fa9437407ecd(network:router_interface) is up
[ DEBUG ] (controller) (node-15.eayun.com): admin_status of port ed5e4596-e7c7-4dda-8b4e-fa9437407ecd(network:router_interface) is up
[ DEBUG ] (controller) (node-15.eayun.com): this port(ed5e4596-e7c7-4dda-8b4e-fa9437407ecd) is network:router_interface port, do not need to check gateway
[ DEBUG ] (controller) (node-15.eayun.com): finish checking port (ed5e4596-e7c7-4dda-8b4e-fa9437407ecd)

```

