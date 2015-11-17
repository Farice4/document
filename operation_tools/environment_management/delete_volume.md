# 删除错误云硬盘

在EayunStack环境运行过程中，可能会出现一些使用cinder命令无法删除的错误volume，此时就需要使用该命令进行强制删除。

> ###### 注意
> 该命令只能在**Controller**节点使用

## 命令格式

```
[controller]$ eayunstack manage volume --help
usage: eayunstack manage volume [-h] [-d] [--id ID]

Volume Management

optional arguments:
  -h, --help            show this help message and exit
  -d, --destroy-volume  Destroy Volume
  --id ID               Volume ID
```

## 删除一个volume

```
# eayunstack manage volume -d --id 8923838d-8d3f-47c0-8dd7-6c03eb957be2
This volume in "available" status, do you really want to destroy it? [yes/no]: yes
This volume has some snapshots , if you want to continue, you must delete the snapshots! Delete the snapshots? [yes/no]: yes
[ INFO  ] (controller) (node-5.eayun.com): Deleting snapshot [u'84f103cb-f571-4f00-b289-75653e46ab6e'] ...
[ INFO  ] (controller) (node-5.eayun.com):    Deleting backend(rbd) snapshots ...
[ INFO  ] (controller) (node-5.eayun.com):    [84f103cb-f571-4f00-b289-75653e46ab6e]Deleting backend snapshot ...
[ INFO  ] (controller) (node-5.eayun.com):    Updating database ...
[ INFO  ] (controller) (node-5.eayun.com):    [84f103cb-f571-4f00-b289-75653e46ab6e]Updating snapshots table ...
[ INFO  ] (controller) (node-5.eayun.com):    [84f103cb-f571-4f00-b289-75653e46ab6e]Updating snapshot quota ...
[ INFO  ] (controller) (node-5.eayun.com): Deleting volume 8923838d-8d3f-47c0-8dd7-6c03eb957be2 ...
[ INFO  ] (controller) (node-5.eayun.com):    Deleting backend(rbd) volume ...
[ INFO  ] (controller) (node-5.eayun.com):    Updating database ...
[ INFO  ] (controller) (node-5.eayun.com):    [8923838d-8d3f-47c0-8dd7-6c03eb957be2]Updating volumes table ...
[ INFO  ] (controller) (node-5.eayun.com):    [8923838d-8d3f-47c0-8dd7-6c03eb957be2]Updating volume quota ...
```
