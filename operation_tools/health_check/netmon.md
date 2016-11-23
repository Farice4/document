# EayunStack网络检测工具

该工具用以检测EayunStack环境中的网络健康情况，目前包括对以下几个部分的检测：
  * Fuel底层网络配置的检测 -- fuelnet
  * OpenStack中虚拟网络连接的检测 -- vnet
  * OpenStack中路由状态的检测 -- l3_router
  * OpenStack中DHCP端口的检测 -- dhcp
  * OpenStack中各种网络组件(路由、防火墙、负载均衡、VPN)状态的检测 -- status

## 命令格式

```
# netmon -h
usage: netmon [-h] [-c CONFIG_FILE] [-p PLUGINS] [--check-status]

EayunStack Networking Debug Tool.

optional arguments:
  -h, --help            show this help message and exit
  -c CONFIG_FILE, --config-file CONFIG_FILE
                        Configuration file location.
  -p PLUGINS, --plugins PLUGINS
                        Plugin to check, use commas to seperate multiple
                        plugins.
  --check-status        Also check network components statuses.
```

### 注意
  * `-p`参数跟着的是多个组件名，使用`,`分隔，可用组件包括上面列出来的fuelnet/vnet/l3_router/dhcp/status，以及all表示检查所有
  * 默认检测所有部分，但不检测Neutron中组件的状态，除非使用`--check-status`参数
  * 目前该命令只在Controller节点上有实际效果
  * 除非使用`status`插件同时指定`--check-status`参数，否则每个节点只检测其之上的网络设备的连接情况

## 与运维工具(eayunstack-tools)的集成

通过将netmon集成到eayunstack-tools中，方便使用现有运维工具的各种报告和记录功能来对网络进行检测

```
# eayunstack doctor netmon -h
usage: eayunstack doctor netmon [-h] [--check-status]

Call netmon

optional arguments:
  -h, --help      show this help message and exit
  --check-status  Also check network components statuses.
```

### 说明
  * 该命令还可以在Fuel节点上执行，此时`--check-status`参数将被忽略，其效果如下：
    - 在一个Controller节点上检测所有组件并检查Neutron中的组件状态
    - 在其他Controller节点上检测所有组件，但不检测Neutron中的组件状态
  * 该命令在非Fuel节点上执行时，表示在该节点上检测所有组件，并根据是否指定`--check-status`参数决定是否检测Neutron中的组件状态

## fuelnet检测

* 检查底层网络配置是否与Fuel的配置文件一致

```
# netmon -p fuelnet
[INFO ][2016-11-23 14:23:21,545]
[INFO ][2016-11-23 14:23:21,546] Checking OpenvSwitch bridge br-enp5s0f1...
[INFO ][2016-11-23 14:23:22,362]   - Port br-enp5s0f1 is on bridge br-enp5s0f1.
[INFO ][2016-11-23 14:23:22,362]   - Port br-enp5s0f1--br-prv is on bridge br-enp5s0f1.
[INFO ][2016-11-23 14:23:22,362]     * Interface br-enp5s0f1--br-prv of port br-enp5s0f1--br-prv on bridge br-enp5s0f1 is connected to peer br-prv--br-enp5s0f1.
[INFO ][2016-11-23 14:23:22,363]   - Port enp5s0f1 is on bridge br-enp5s0f1.
[INFO ][2016-11-23 14:23:22,363]
(...)
```

## vnet检测

* 检查OpenStack中L2虚拟网络的连接情况

```
# netmon -p vnet
[INFO ][2016-11-23 14:25:02,267]
[INFO ][2016-11-23 14:25:02,268] Checking OpenvSwitch bridge br-prv...
[INFO ][2016-11-23 14:25:02,928]   - Port phy-br-prv is on bridge br-prv.
[INFO ][2016-11-23 14:25:02,928]     * Interface phy-br-prv of port phy-br-prv on bridge br-prv is connected to peer int-br-prv.
[INFO ][2016-11-23 14:25:02,929]
[INFO ][2016-11-23 14:25:02,929] Checking OpenvSwitch bridge br-int...
[INFO ][2016-11-23 14:25:02,929]   - Port int-br-prv is on bridge br-int.
[INFO ][2016-11-23 14:25:02,929]     * Interface int-br-prv of port int-br-prv on bridge br-int is connected to peer phy-br-prv.
```

## l3_router检测

* 检查运行在当前节点之上的OpenStack中定义的路由状态

```
# netmon -p l3_router
[INFO ][2016-11-23 14:27:09,408]
[INFO ][2016-11-23 14:27:09,408] Checking L3 router b158bae4-6201-457e-877e-4180520d56f4...
[INFO ][2016-11-23 14:27:09,408] Checking gateway port 6fe38d63-9581-4cd4-bd96-8cb2e24d08d1 of router b158bae4-6201-457e-877e-4180520d56f4...
[INFO ][2016-11-23 14:27:10,602]   - Port qg-6fe38d63-95 is on bridge br-ex.
[INFO ][2016-11-23 14:27:10,604]   - Router b158bae4-6201-457e-877e-4180520d56f4 can connect to 182.150.27.112:9090.
[INFO ][2016-11-23 14:27:11,417]   - Device qg-6fe38d63-95 is running with the following IP addresses configured: [u'25.0.0.164/24'].
[INFO ][2016-11-23 14:27:11,417] Checking router ports on router b158bae4-6201-457e-877e-4180520d56f4...
[INFO ][2016-11-23 14:27:11,418] Checking router port ea7064d3-d017-4489-9aee-7937d6d0d407 of subnet bc9d4d92-1d8b-4fec-babe-078834d68d7b on router b158bae4-6201-457e-877e-4180520d56f4...
[INFO ][2016-11-23 14:27:11,418]   - Port qr-ea7064d3-d0 is on bridge br-int.
[INFO ][2016-11-23 14:27:11,418]   - Port qr-ea7064d3-d0 on bridge br-int is configured with tag 2.
[INFO ][2016-11-23 14:27:11,513]   - Device qr-ea7064d3-d0 is running with the following IP addresses configured: [u'192.168.1.1/24'].
```

## dhcp检测

* 检查OpenStack中定义的DHCP服务的端口状态
* dhcp-agent需要运行在当前节点之上

```
# netmon -p dhcp
[INFO ][2016-11-23 14:28:51,847]
[INFO ][2016-11-23 14:28:51,847] Checking DHCP port tapdd3864a1-01 for network b62a2b5f-d295-4fff-8daf-eee9c590a94b...
[INFO ][2016-11-23 14:28:52,645]   - Port tapdd3864a1-01 is on bridge br-int.
[INFO ][2016-11-23 14:28:52,646]   - Port tapdd3864a1-01 on bridge br-int is configured with tag 25.
[INFO ][2016-11-23 14:28:52,646] Checking DHCP port IP address for port tapdd3864a1-01 of subnet 63ee7834-2bce-4c60-8426-ba4b1ee12cbb...
[INFO ][2016-11-23 14:28:52,670]   - Device tapdd3864a1-01 is running with the following IP addresses configured: [u'55.55.55.100/24'].
[INFO ][2016-11-23 14:28:52,670]
(...)
```

## Neutron组件状态检测

* 检测Neutron中的网络组件状态是否正常
* 此部分只可以在Controller节点上执行

```
# netmon -p status --check-status
[INFO ][2016-11-23 14:30:27,311] Firewall 02ab6720-06b3-483c-aa9e-013838d460fb is enabled and active.
(...)
[INFO ][2016-11-23 14:30:27,311] Router 00d68f16-8206-4a35-9bb2-7ee6d491da19 is enabled and active.
(...)
[ERROR][2016-11-23 14:30:27,318] LB member 14e764bc-541a-49d2-897e-addd5cc943ae is enabled but its status is INACTIVE!
[INFO ][2016-11-23 14:30:27,318] LB member 15a63a49-43f5-44bc-8ab6-0780c15a8815 is enabled and active.
(...)
[INFO ][2016-11-23 14:30:27,320] LB pool 00b19929-bf81-48a4-9522-baabf76a2f53 is enabled and active.
(...)
[INFO ][2016-11-23 14:30:27,322] LB vip 20aa16ca-d6bd-42b0-b949-9ca875686ad2 is enabled and active.
(...)
[INFO ][2016-11-23 14:30:27,325] IPsec site connection 9f227d11-67ea-4de8-9d28-5bbbdf4d8093 is enabled and active.
(...)
[INFO ][2016-11-23 14:30:27,325] Skipping check of the status of VPN service 391d95dd-03e3-4c7c-be4c-b6ca0cc361c1...
[INFO ][2016-11-23 14:30:27,326] VPN service 4847daf8-a0bb-4f67-93c7-29b6b3242330 is enabled and active.
(...)
```
