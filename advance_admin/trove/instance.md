# 数据库实例管理

## 查看数据库实例列表

### 命令行使用

``` bash
  trove list
```

### 示例

``` bash
  # trove list
  +--------------------------------------+----------+-----------+-------------------+--------+-----------+------+
  | ID                                   | Name     | Datastore | Datastore Version | Status | Flavor ID | Size |
  +--------------------------------------+----------+-----------+-------------------+--------+-----------+------+
  | a17461ab-6c97-4abf-8890-9f8b10886ecc | master_1 | mysql     | 5.5               | ACTIVE | 6         |    1 |
  | a970995e-198f-4ac7-ae8c-42956a8a646d | slave_1  | mysql     | 5.5               | ACTIVE | 6         |    1 |
  +--------------------------------------+----------+-----------+-------------------+--------+-----------+------+
```

## 启动新的数据库实例

### 命令行使用

``` bash
  trove create <name> <flavor_id>
                      [--size <size>]
                      [--databases <databases> [<databases> ...]]
                      [--users <users> [<users> ...]] [--backup <backup>]
                      [--availability_zone <availability_zone>]
                      [--datastore <datastore>]
                      [--datastore_version <datastore_version>]
                      [--nic <net-id=net-uuid,v4-fixed-ip=ip-addr,port-id=port-uuid>]
                      [--configuration <configuration>]
                      [--replica_of <source_id>]
```

### 示例

创建数据库实例，名为 master, 规格 id 是 6, 数据库引擎是 mysql，版本是 5.5, 数据盘 10G

``` bash
  # trove create master 6 --datastore mysql --datastore_version 5.5 --size 10
```

## 删除数据库实例

### 命令行使用

``` bash
  trove delete <instance>
```

### 示例

``` bash
  # trove delete a17461ab-6c97-4abf-8890-9f8b10886ecc
```

## 查看数据库实例详情

### 命令行使用

``` bash
  trove show <instance>
```

### 示例

``` bash
  # trove show a17461ab-6c97-4abf-8890-9f8b10886ecc
  +-------------------+--------------------------------------+
  | Property          | Value                                |
  +-------------------+--------------------------------------+
  | created           | 2016-12-28T02:19:23                  |
  | datastore         | mysql                                |
  | datastore_version | 5.5                                  |
  | flavor            | 6                                    |
  | id                | a17461ab-6c97-4abf-8890-9f8b10886ecc |
  | ip                | 172.16.0.153, 25.0.0.192             |
  | name              | master_1                             |
  | replicas          | a970995e-198f-4ac7-ae8c-42956a8a646d |
  | status            | ACTIVE                               |
  | updated           | 2016-12-28T02:19:33                  |
  | volume            | 1                                    |
  | volume_used       | 0.1                                  |
  +-------------------+--------------------------------------+
```

## 重启数据库实例

有的情况需要重启数据库实例，如动态修改了数据库需要重启才生效的配置。

### 命令行使用

``` bash
  trove restart <instance>
```

### 示例

``` bash
  # trove restart a17461ab-6c97-4abf-8890-9f8b10886ecc
```

## 扩展数据库实例规格

### 命令行使用

``` bash
  # trove resize-instance <instance> <flavor_id>
```

### 示例

数据库实例规格扩展为 id 为 7 的 flavor

``` bash
  # trove resize-instance a17461ab-6c97-4abf-8890-9f8b10886ecc 7
```

## 扩展数据库实例数据盘大小

### 命令行使用

``` bash
  trove resize-volume <instance> <size>
```

### 示例

将数据库实例的数据盘扩展为 20G

``` bash
  # trove resize-volume a17461ab-6c97-4abf-8890-9f8b10886ecc 20
```
