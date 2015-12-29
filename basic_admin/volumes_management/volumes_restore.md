# 恢复磁盘卷

### 通过命令恢复磁盘卷

*  恢复磁盘卷，执行如下命令

> ```cinder backup-restore [--volume-id <volume>] <backup>```

* 查看磁盘卷，执行如下命令

> ```cinder backup-list```


### 示例

* 查看磁盘卷备份信息

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

* 恢复磁盘卷

```
# cinder backup-restore b85c7d9c-9726-47c1-9f03-5d8ea6675e36
```

恢复完成后通过对磁盘卷挂载，查询云主机挂载卷数据是否恢复
> * 恢复时确保openstack-cinder-backup处于启动状态
> * 恢复时需要注意恢复使用的是cinder backup-list 查询的id恢复

