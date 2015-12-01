# FWaaS 选择目标路由功能
原来在 FWaaS 功能中所设置的防火墙，会应用到该租户之下的所有路由器之上，对所有路由都生效。

本功能提供将防火墙只绑定到某些特定路由器上的功能，使得对防火墙功能能够有更细粒度的管理，提高该功能的灵活性。
  * 防火墙设置了目标路由：只有在目标路由上该防火墙才生效
  * 防火墙未设置目标路由：该防火墙在该同一租户的所有路由上都生效

## 使用方法
1. 创建防火墙时指定目标路由，
指定 --fw_target_routers 参数，将要设置防火墙的路由器 id 以如下方式提供命令行参数
    ```
    neutron firewall-create 创建防火墙的参数 --fw_target_routers list=true router_id1 router_id2...
    ```
2. 更新防火墙的目标路由列表，
同样是指定 --fw_target_routers 参数，如下
    ```
    neutron firewall-update 更新防火墙的参数 --fw_target_routers list=true new_router_id1 new_router_id2...
    ```
3. 清空防火墙的目标路由列表，
执行此命令将使得该防火墙将在该租户下的所有路由都生效，命令如下
    ```
    neutron firewall-update 更新防火墙的参数 --fw_target_routers action=clear
    ```
