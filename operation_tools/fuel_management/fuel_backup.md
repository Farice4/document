# Fuel节点备份

该命令主要用来对Fuel节点进行备份。

## 命令格式

```
[fuel]$ eayunstack fuel backup --help
usage: eayunstack fuel backup [-h] [-n] [-l]

Fuel Backup

optional arguments:
  -h, --help  show this help message and exit
  -n, --new   Create A New Backup
  -l, --list  List All Backups
```

## 创建新的备份

```
[fuel]$ eayunstack fuel backup -n
[ INFO  ] Starting backup ...
[ INFO  ] Backup is in progress, Please wait ...
[ INFO  ] Backup successfully completed!

You can use "eayunstack fuel backup [ -l | --list ]" to list your backups
```

## 列出所有备份

```
[fuel]$ eayunstack fuel backup -l
+----+------------------+-------------------------------------+
| ID |   Backup Time    |             Backup File             |
+----+------------------+-------------------------------------+
| 1  | 2015-04-19 12:05 | fuel_backup_2015-04-19_1205.tar.lrz |
| 2  | 2015-04-19 12:09 | fuel_backup_2015-04-19_1209.tar.lrz |
| 3  | 2015-04-19 12:12 | fuel_backup_2015-04-19_1212.tar.lrz |
+----+------------------+-------------------------------------+
```
