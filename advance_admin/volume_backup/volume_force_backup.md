# 云硬盘强制备份

## 说明

云硬盘备份是要求云硬盘的状态为 “available”，即在云硬盘被云主机使用的情况下是无法进行备份的。

云硬盘备份/恢复的基本操作参见：

* [备份磁盘卷](../../basic_admin/volumes_management/volumes_backup.md)
* [恢复磁盘卷](../../basic_admin/volumes_management/volumes_restore.md)

云硬盘强制备份可以实现云硬盘的在线备份，不需要提前将云硬盘从云主机中解绑。

## 命令行使用

强制备份需要使用 Cinder API v2 ，使用命令行时，可以通过 export OS_VOLUME_API_VERSION=2 或者在命令行中增加 --os-volume-api-version 2 来指定使用 Cinder API v2 。

> ``` cinder backup-create [--container <container>] [--name <name>]
                           [--description <description>] [--force]
                           <volume>
```

## 示例

* 强制备份云硬盘

备份磁盘卷之前需要通过cinder list 查询需要备份磁盘卷的 ID 或者磁盘卷名称

> 下面的示例以一块已经绑定的云主机的云硬盘为例

```
# cinder list
+--------------------------------------+--------+---------------+------+-------------+-----------+--------------------------------------+
|                  ID                  | Status |      Name     | Size | Volume Type |  Bootable |             Attached to              |
+--------------------------------------+--------+---------------+------+-------------+-----------+--------------------------------------+
| 5c812f64-76de-41fb-bce9-5448e25d599d | in-use | TestVM_volume |  1   |     None    |   false   | f1864dfd-90ab-432c-a852-5180dea74dc3 |
+--------------------------------------+--------+---------------+------+-------------+-----------+--------------------------------------+
```

通过cinder backup-create 备份

```
# cinder --os-volume-api-version 2 backup-create --force --name TestVM_volue_Backup TestVM_volume
+-----------+--------------------------------------+
|  Property |                Value                 |
+-----------+--------------------------------------+
|     id    | e31a6115-d19d-4334-943f-96eca4806739 |
|    name   |         TestVM_volue_Backup          |
| volume_id | 5c812f64-76de-41fb-bce9-5448e25d599d |
+-----------+--------------------------------------+
```

* 查看备份结果

```
# cinder backup-list
+--------------------------------------+--------------------------------------+-----------+---------------------+------+--------------+----------------+
|                  ID                  |              Volume ID               |   Status  |         Name        | Size | Object Count |   Container    |
+--------------------------------------+--------------------------------------+-----------+---------------------+------+--------------+----------------+
| e31a6115-d19d-4334-943f-96eca4806739 | 5c812f64-76de-41fb-bce9-5448e25d599d | available | TestVM_volue_Backup |  1   |     None     | volumes-backup |
+--------------------------------------+--------------------------------------+-----------+---------------------+------+--------------+----------------+
```

* 查看备份磁盘卷详细信息

```
# cinder backup-show e31a6115-d19d-4334-943f-96eca4806739
+-------------------+--------------------------------------+
|      Property     |                Value                 |
+-------------------+--------------------------------------+
| availability_zone |                 nova                 |
|     container     |            volumes-backup            |
|     created_at    |      2016-12-12T04:08:48.000000      |
|    description    |                 None                 |
|    fail_reason    |                 None                 |
|         id        | e31a6115-d19d-4334-943f-96eca4806739 |
|        name       |         TestVM_volue_Backup          |
|    object_count   |                 None                 |
|        size       |                  1                   |
|       status      |              available               |
|     volume_id     | 5c812f64-76de-41fb-bce9-5448e25d599d |
+-------------------+--------------------------------------+
```
