# 数据库配置组管理

## 查看数据库配置组列表

### 命令行使用

``` bash
  trove configuration-list
```

### 示例

``` bash
  # trove configuration-list
  +--------------------------------------+--------+-------------+----------------+------------------------+
  | ID                                   | Name   | Description | Datastore Name | Datastore Version Name |
  +--------------------------------------+--------+-------------+----------------+------------------------+
  | ef853d4e-6ab8-4463-9bea-f0d6f846cce9 | cfg_01 | the cfg_01  | mysql          | 5.5                    |
  +--------------------------------------+--------+-------------+----------------+------------------------+
```

## 查看数据库配置组的可用的配置项列表

### 命令行使用

``` bash
  trove configuration-parameter-list <datastore_version>
                                            [--datastore <datastore>]
```

### 示例

``` bash
  # trove configuration-parameter-list 5.5 --datastore mysql
  +--------------------------------+---------+----------+----------------------+------------------+
  | Name                           | Type    | Min Size | Max Size             | Restart Required |
  +--------------------------------+---------+----------+----------------------+------------------+
  | auto_increment_increment       | integer | 1        | 65535                |            False |
  | auto_increment_offset          | integer | 1        | 65535                |            False |
  | autocommit                     | integer | 0        | 1                    |            False |
  | bulk_insert_buffer_size        | integer | 0        | 18446744073709547520 |            False |
  | character_set_client           | string  |          |                      |            False |
  | character_set_connection       | string  |          |                      |            False |
  | character_set_database         | string  |          |                      |            False |
  | character_set_filesystem       | string  |          |                      |            False |
  | character_set_results          | string  |          |                      |            False |
  | character_set_server           | string  |          |                      |            False |
  | collation_connection           | string  |          |                      |            False |
  | collation_database             | string  |          |                      |            False |
  | collation_server               | string  |          |                      |            False |
  | connect_timeout                | integer | 1        | 65535                |            False |
  | expire_logs_days               | integer | 1        | 65535                |            False |
  | innodb_buffer_pool_size        | integer | 0        | 68719476736          |             True |
  | innodb_file_per_table          | integer | 0        | 1                    |             True |
  | innodb_flush_log_at_trx_commit | integer | 0        | 2                    |            False |
  | innodb_log_buffer_size         | integer | 1048576  | 4294967296           |             True |
  | innodb_open_files              | integer | 10       | 4294967296           |             True |
  | innodb_thread_concurrency      | integer | 0        | 1000                 |            False |
  | interactive_timeout            | integer | 1        | 65535                |            False |
  | join_buffer_size               | integer | 0        | 4294967296           |            False |
  | key_buffer_size                | integer | 0        | 4294967296           |            False |
  | local_infile                   | integer | 0        | 1                    |            False |
  | max_allowed_packet             | integer | 1024     | 1073741824           |            False |
  | max_connect_errors             | integer | 1        | 18446744073709547520 |            False |
  | max_connections                | integer | 1        | 65535                |            False |
  | max_user_connections           | integer | 1        | 100000               |            False |
  | myisam_sort_buffer_size        | integer | 4        | 18446744073709547520 |            False |
  | server_id                      | integer | 1        | 100000               |             True |
  | sort_buffer_size               | integer | 32768    | 18446744073709547520 |            False |
  | sync_binlog                    | integer | 0        | 18446744073709547520 |            False |
  | wait_timeout                   | integer | 1        | 31536000             |            False |
  +--------------------------------+---------+----------+----------------------+------------------+
```

## 查看配置项详情

### 命令行使用

``` bash
  trove configuration-parameter-show <datastore_version> <parameter>
                                            [--datastore <datastore>]
```

### 示例

``` bash
  # trove configuration-parameter-show 5.5 max_connections --datastore mysql
  +----------------------+--------------------------------------+
  | Property             | Value                                |
  +----------------------+--------------------------------------+
  | datastore_version_id | 35b4c566-935c-4606-b077-20b90f2bf5df |
  | max_size             | 65535                                |
  | min_size             | 1                                    |
  | name                 | max_connections                      |
  | restart_required     | False                                |
  | type                 | integer                              |
  +----------------------+--------------------------------------+
```

## 创建数据库配置组

### 命令行使用

``` bash
  trove configuration-create <name> <values>
                                    [--datastore <datastore>]
                                    [--datastore_version <datastore_version>]
                                    [--description <description>]
```

### 示例

``` bash
  # trove configuration-create cfg_01 '{"sync_binlog": 1,"max_connections": 80}' --datastore mysql --datastore_version 5.5 --description "the cfg_01"
  +------------------------+-------------------------------------------+
  | Property               | Value                                     |
  +------------------------+-------------------------------------------+
  | created                | 2016-12-30T03:30:47                       |
  | datastore_name         | mysql                                     |
  | datastore_version_id   | 35b4c566-935c-4606-b077-20b90f2bf5df      |
  | datastore_version_name | 5.5                                       |
  | description            | the cfg_01                                |
  | id                     | ef853d4e-6ab8-4463-9bea-f0d6f846cce9      |
  | instance_count         | 0                                         |
  | name                   | cfg_01                                    |
  | updated                | 2016-12-30T03:30:47                       |
  | values                 | {"sync_binlog": 1, "max_connections": 80} |
  +------------------------+-------------------------------------------+
```

## 删除数据库配置组

### 命令行使用

``` bash
  trove configuration-delete <configuration_group>
```

### 示例

``` bash
  trove configuration-delete 35b4c566-935c-4606-b077-20b90f2bf5d
```

## 查看数据库配置组详情

### 命令行使用

``` bash
  trove configuration-show <configuration_group>
```

### 示例

``` bash
  # trove configuration-show ef853d4e-6ab8-4463-9bea-f0d6f846cce9
  +------------------------+-------------------------------------------+
  | Property               | Value                                     |
  +------------------------+-------------------------------------------+
  | created                | 2016-12-30T03:30:47                       |
  | datastore_name         | mysql                                     |
  | datastore_version_name | 5.5                                       |
  | description            | the cfg_01                                |
  | id                     | ef853d4e-6ab8-4463-9bea-f0d6f846cce9      |
  | instance_count         | 0                                         |
  | name                   | cfg_01                                    |
  | updated                | 2016-12-30T03:30:47                       |
  | values                 | {"sync_binlog": 1, "max_connections": 80} |
  +------------------------+-------------------------------------------+
```

## 更新数据库配置组

### 命令行使用

``` bash
  trove configuration-update <configuration_group> <values>
                                    [--name <name>]
                                    [--description <description>]
```

### 示例

``` bash
  # trove configuration-update ef853d4e-6ab8-4463-9bea-f0d6f846cce9 '{"max_connections": 80}' --description "the cfg_01..."
```

## 给数据库配置组添加配置

### 命令行使用

``` bash
  trove configuration-patch <configuration_group> <values>
```

### 示例

``` bash
  # trove configuration-patch ef853d4e-6ab8-4463-9bea-f0d6f846cce9 '{"collation_database": "utf8"}'
```

## 查看数据库实例的默认配置

### 命令行使用

``` bash
  trove configuration-default <instance>
```

### 示例

``` bash
  # trove configuration-default a17461ab-6c97-4abf-8890-9f8b10886ecc
  +---------------------------+----------------------------+
  | Property                  | Value                      |
  +---------------------------+----------------------------+
  | basedir                   | /usr                       |
  | connect_timeout           | 15                         |
  | datadir                   | /var/lib/mysql             |
  | default_storage_engine    | innodb                     |
  | innodb_buffer_pool_size   | 150M                       |
  | innodb_data_file_path     | ibdata1:10M:autoextend     |
  | innodb_file_per_table     | 1                          |
  | innodb_log_buffer_size    | 25M                        |
  | innodb_log_file_size      | 50M                        |
  | innodb_log_files_in_group | 2                          |
  | join_buffer_size          | 1M                         |
  | key_buffer_size           | 50M                        |
  | local-infile              | 0                          |
  | max_allowed_packet        | 1024K                      |
  | max_connections           | 100                        |
  | max_heap_table_size       | 16M                        |
  | max_user_connections      | 100                        |
  | myisam-recover            | BACKUP                     |
  | open_files_limit          | 512                        |
  | pid_file                  | /var/run/mysqld/mysqld.pid |
  | port                      | 3306                       |
  | query_cache_limit         | 1M                         |
  | query_cache_size          | 8M                         |
  | query_cache_type          | 1                          |
  | read_buffer_size          | 512K                       |
  | read_rnd_buffer_size      | 512K                       |
  | server_id                 | 79499086                   |
  | skip-external-locking     | 1                          |
  | sort_buffer_size          | 1M                         |
  | table_definition_cache    | 256                        |
  | table_open_cache          | 256                        |
  | thread_cache_size         | 4                          |
  | thread_stack              | 192K                       |
  | tmp_table_size            | 16M                        |
  | tmpdir                    | /var/tmp                   |
  | user                      | mysql                      |
  | wait_timeout              | 120                        |
  +---------------------------+----------------------------+
```

## 挂载配置组到数据库实例

### 命令行使用

``` bash
  trove configuration-attach <instance> <configuration>
```

### 示例

``` bash
  # trove configuration-attach a17461ab-6c97-4abf-8890-9f8b10886ecc ef853d4e-6ab8-4463-9bea-f0d6f846cce9
```

## 卸载配置组从数据库实例

### 命令行使用

``` bash
  trove configuration-detach <instance>
```

### 示例

``` bash
  # trove configuration-detach a17461ab-6c97-4abf-8890-9f8b10886ecc
```
