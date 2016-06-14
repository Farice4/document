# Fuel节点管理命令

该命令用于对Fuel节点进行备份与恢复，以及在环境部署时配置Ceph Cluster网络。

## 命令格式

```
[fuel]# eayunstack fuel --help
usage: eayunstack fuel [-h] COMMAND ...

EayunStack Fuel Management

optional arguments:
  -h, --help            show this help message and exit

Commands:
  COMMAND               DESCRIPTION
    restore             Fuel Restore
    backup              Fuel Backup
    deployment_monitor_plugins
                        Deploy fuel monitor plugins.
```