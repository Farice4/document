# 环境管理命令

## 简介

该命令用来对EayunStack环境中的一些对象进行管理操作，包括上传AMI镜像、删除错误的云硬盘、删除错误的云主机。

## 命令格式

> **注意**
>
> 该命令仅可以在Controller节点使用


```
(controller)# eayunstack manage --help
usage: eayunstack manage [-h] COMMAND ...

EayunStack Management

optional arguments:
  -h, --help  show this help message and exit

Commands:
  COMMAND     DESCRIPTION
    volume    Volume Management
    ami       AMI Image Management
    instance  Instance Management
```
