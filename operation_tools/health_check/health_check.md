# 健康检测命令

## 简介

该命令用来对EayunStack环境的中的基本环境、OpenStack组件、集群运行状态、OpenStack网络进行快速检查，便于管理员在环境出现问题时对故障点进行快速定位，缩短故障排除的时间。

## 命令格式

```
# eayunstack doctor --help
usage: eayunstack doctor [-h] COMMAND ...

EayunStack Doctor

optional arguments:
  -h, --help  show this help message and exit

Commands:
  COMMAND     DESCRIPTION
    all       Check All Object
    cls       Check cluster
    net       Check openstack network. e.g. virtual router
    env       Check Environment Object
    stack     Check OpenStack Compent
```
