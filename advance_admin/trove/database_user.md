# 数据库用户管理

## 查看数据库用户列表

### 命令行使用

``` bash
  trove user-list <instance>
```

### 示例

``` bash
  # trove user-list a17461ab-6c97-4abf-8890-9f8b10886ecc
  +----------------+------+-----------+
  | Name           | Host | Databases |
  +----------------+------+-----------+
  | admin          | %    | mydb      |
  | slave_222f03cc | %    |           |
  +----------------+------+-----------+
```

## 创建数据库用户

### 命令行使用

``` bash
  trove user-create <instance> <name> <password>
                           [--host <host>]
                           [--databases <databases> [<databases> ...]]
```

### 示例

``` bash
  # trove user-create a17461ab-6c97-4abf-8890-9f8b10886ecc stack stack@2016 --host % --databases mydb db_01
```

## 查看数据库用户详情

### 命令行使用

``` bash
  trove user-show [--host <host>] <instance> <name>
```

### 示例

``` bash
  # trove user-show a17461ab-6c97-4abf-8890-9f8b10886ecc stack
  +-----------+-------------------------------------------+
  | Property  | Value                                     |
  +-----------+-------------------------------------------+
  | databases | [{u'name': u'db_01'}, {u'name': u'mydb'}] |
  | host      | %                                         |
  | name      | stack                                     |
  +-----------+-------------------------------------------+
```

## 查看用户权限

### 命令行使用

``` bash
  trove user-show-access [--host <host>] <instance> <name>
```

### 示例

``` bash
  # trove user-show-access a17461ab-6c97-4abf-8890-9f8b10886ecc stack
  +-------+
  | Name  |
  +-------+
  | db_01 |
  | mydb  |
  +-------+
```

## 授于用户数据库访问权限

### 命令行使用

``` bash
  trove user-grant-access <instance> <name> <databases> [<databases> ...]
                                 [--host <host>]
```

### 示例

``` bash
  # trove user-grant-access a17461ab-6c97-4abf-8890-9f8b10886ecc stack mydb1
```

## 撤销用户数据库访问权限

### 命令行使用

``` bash
  trove user-revoke-access [--host <host>] <instance> <name> <database>
```

### 示例

``` bash
  # trove user-revoke-access a17461ab-6c97-4abf-8890-9f8b10886ecc stack mydb1
```

## 更新用户属性

### 命令行使用

``` bash
  trove user-update-attributes <instance> <name>
                                      [--host <host>] [--new_name <new_name>]
                                      [--new_password <new_password>]
                                      [--new_host <new_host>]
```

### 示例

``` bash
  # trove user-update-attributes a17461ab-6c97-4abf-8890-9f8b10886ecc stack --host % --new_name stack_01 --new_password stack_01@2016 --new_host '110.111.127.%'
```
