# 端口映射功能介绍
本功能提供将路由器上的某个端口映射到内网某台机器的某个端口的功能，从而减少对浮动地址数量的需求，满足简单应用环境下的需要。

## 使用方法
1. 列出已存在的端口映射项目，
使用如下命令
    ```
    neutron portmapping-list
    ```
2. 显示某一个端口映射条目的详细信息，
使用如下命令
    ```
    neutron portmapping-show PORTMAPPING_ID
    ```
3. 创建一个新的端口映射
    ```
    neutron portmapping-create [-h] [-f {html,json,shell,table,value,yaml}]
                            [-c COLUMN] [--max-width <integer>]
                            [--prefix PREFIX]
                            [--request-format {json,xml}]
                            [--tenant-id TENANT_ID] [--name NAME]
                            [--protocol PROTOCOL]
                            ROUTER_ID ROUTER_PORT
                            DESTINATION_IP DESTINATION_PORT
    ```
  - 必需参数说明
    * ROUTER_ID: 要进行端口映射的路由 id
    * ROUTER_PORT: 被映射的路由端口
    * DESTINATION_IP: 映射的目标地址
    * DESTINATION_PORT: 映射的目标端口
  - 可选参数说明
    * --protocol PROTOCOL: 映射的协议，支持 tcp 和 udp，默认为 tcp
    * --name NAME: 可以为该端口映射设置名称
4. 更新一个端口映射项目(只支持更新端口映射名称和管理员状态)
    ```
    neutron portmapping-update [-h] [--request-format {json,xml}]
                            [--name NAME]
                            PORTMAPPING
    ```
  - 必需参数说明
    * PORTMAPPING: 要更新的端口映射项目的 id
  - 可选参数说明
    * --name NAME: 更新该端口映射项目的名称
    * --admin_state_up True/False: 更新该端口映射项目的管理员状态
5. 删除端口映射
    ```
    neutron portmapping-delete [-h] [--request-format {json,xml}]
                            PORTMAPPING
    ```
  - 必需参数说明
    * PORTMAPPING: 要删除的端口映射项目的 id
