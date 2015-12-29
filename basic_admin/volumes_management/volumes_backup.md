# 备份磁盘卷

### 通过命令备份磁盘卷

*  备份磁盘卷，执行如命令

> ``` cinder backup-create [--container <container>]
    [--display-name <display-name>]
    [--display-description <display-description>] <volume>
```

* 查看磁盘卷命令

   cinder list

* 查看备份磁盘卷命令

   cinder backup-list


### 示例

* 备份磁盘卷

备份磁盘卷之前需要通过cinder list 查询需要备份磁盘卷的ID or 磁盘卷名称

通过cinder backup-create 备份

```
# cinder backup-create TestVM_volume --display-name TestVM_volume_Backup
+-----------+--------------------------------------+
|  Property |                Value                 |
+-----------+--------------------------------------+
|     id    | b85c7d9c-9726-47c1-9f03-5d8ea6675e36 |
|    name   |         TestVM_volume_Backup         |
| volume_id | 8e98c878-c6e3-40db-9843-9a8dad125243 |
+-----------+--------------------------------------+

```

* 查看备份结果

```
# cinder backup-list
+--------------------------------------+--------------------------------------+-----------+----------------------+------+--------------+----------------+
|                  ID                  |              Volume ID               |   Status  |         Name         | Size | Object Count |   Container    |
+--------------------------------------+--------------------------------------+-----------+----------------------+------+--------------+----------------+
| 396e20dd-bb10-4fdc-98a7-ae6f71625c26 | 09d0cd58-c1fb-4930-a4db-40e35bc94c2f | available |         None         |  6   |     None     | volumes-backup |
| b85c7d9c-9726-47c1-9f03-5d8ea6675e36 | 8e98c878-c6e3-40db-9843-9a8dad125243 | available | TestVM_volume_Backup |  1   |     None     | volumes-backup |
| ba701520-2606-4d52-aa93-871bc7e9affd | cbd0f9c0-6d9d-4d0b-9f0e-f7d460f084ac | available |     backup-test      |  1   |     None     | volumes-backup |
+--------------------------------------+--------------------------------------+-----------+----------------------+------+--------------+----------------+

```
* 查看备份磁盘卷详细信息

```
# cinder backup-show TestVM_volume_Backup
+-------------------+--------------------------------------+
|      Property     |                Value                 |
+-------------------+--------------------------------------+
| availability_zone |                 nova                 |
|     container     |            volumes-backup            |
|     created_at    |      2015-05-22T08:44:46.000000      |
|    description    |                 None                 |
|    fail_reason    |                 None                 |
|         id        | b85c7d9c-9726-47c1-9f03-5d8ea6675e36 |
|        name       |         TestVM_volume_Backup         |
|    object_count   |                 None                 |
|        size       |                  1                   |
|       status      |              available               |
|     volume_id     | 8e98c878-c6e3-40db-9843-9a8dad125243 |
+-------------------+--------------------------------------+

```

> * 备份磁盘卷时确保openstack-cinder-backup服务处于启动状态
> * 备份时卷状态必须是"available" 无云主机使用
