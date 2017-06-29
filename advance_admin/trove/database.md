# 数据库管理

## 查看数据库列表

查询数据库实例的数据库列表。

### 命令行使用

``` bash
  trove database-list <instance>
```

### 示例

``` bash
  # trove database-list a17461ab-6c97-4abf-8890-9f8b10886ecc
  +--------------------+
  | Name               |
  +--------------------+
  | mydb               |
  | mydb1              |
  | performance_schema |
  | test               |
  +--------------------+
```

## 创建数据库

### 命令行使用

``` bash
  trove database-create <instance> <name>
                               [--character_set <character_set>]
                               [--collate <collate>]
```

### 示例

``` bash
  # trove database-create a17461ab-6c97-4abf-8890-9f8b10886ecc db_01
```

## 删除数据库

### 命令行使用

``` bash
  trove database-delete <instance> <database>
```

### 示例

``` bash
  # trove database-delete a17461ab-6c97-4abf-8890-9f8b10886ecc db_01
```
