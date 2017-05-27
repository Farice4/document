# 节点

节点就是通过启动配置创建的云主机，可以单独存在也可以加入到集群当中。它是弹性伸缩的最小单位，集群的伸缩就是通过增加或者删除节点来完成。

## 创建节点

创建节点的命令为`senlin node-create`，在创建节点时必须需指定名称和启动配置，可以选择是否将创建好的节点加入集群或者添加元数据。

```
$ senlin node-create -p cirros -c cluster_1 -M key=value node_1
+---------------+--------------------------------------+
| Property      | Value                                |
+---------------+--------------------------------------+
| cluster_id    | 74017671-f945-4843-859f-05492195b8d7 |
| created_at    | -                                    |
| data          | {}                                   |
| dependents    | {}                                   |
| details       | -                                    |
| id            | f3234289-388d-430b-93ce-b0ec5aec5a0c |
| index         | 1                                    |
| init_at       | 2017-05-27T02:20:44Z                 |
| location      | -                                    |
| metadata      | {                                    |
|               |   "key": "value"                     |
|               | }                                    |
| name          | node_1                               |
| physical_id   | -                                    |
| profile_id    | e5884817-3f08-4468-acd7-a8da836a4be4 |
| profile_name  | cirros                               |
| project_id    | 68330cdfaf204bc7961405e8d1e62e5d     |
| role          | -                                    |
| status        | INIT                                 |
| status_reason | Initializing                         |
| updated_at    | -                                    |
| user_id       | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+---------------+--------------------------------------+
```

## 查看节点

如果已经创建了节点，可以使用`senlin node-show`命令查看节点的信息。使用`-D`参数可以查看节点的物理资源详细信息。

```
$ senlin node-show -D node_1
+---------------+--------------------------------------------------------------------------------------+
| Property      | Value                                                                                |
+---------------+--------------------------------------------------------------------------------------+
| cluster_id    | 74017671-f945-4843-859f-05492195b8d7                                                 |
| created_at    | 2017-05-27T02:23:12Z                                                                 |
| data          | {}                                                                                   |
| dependents    | {}                                                                                   |
| details       | +------------------+---------------------------------------------------------------+ |
|               | | property         | value                                                         | |
|               | +------------------+---------------------------------------------------------------+ |
|               | | addresses        | {                                                             | |
|               | |                  |   "private": [                                                | |
|               | |                  |     {                                                         | |
|               | |                  |       "OS-EXT-IPS-MAC:mac_addr": "fa:16:3e:eb:7b:d9",         | |
|               | |                  |       "version": 4,                                           | |
|               | |                  |       "addr": "10.0.0.86",                                    | |
|               | |                  |       "OS-EXT-IPS:type": "fixed"                              | |
|               | |                  |     }                                                         | |
|               | |                  |   ]                                                           | |
|               | |                  | }                                                             | |
|               | | flavor           | 0                                                             | |
|               | | id               | 3b5c414a-a20c-47eb-86ea-ef5ee5a89fa7                          | |
|               | | image            | e32bc506-a19f-4825-ac94-2faf805428c7                          | |
|               | | key_name         | null                                                          | |
|               | | metadata         | {                                                             | |
|               | |                  |   "cluster_node_id": "f3234289-388d-430b-93ce-b0ec5aec5a0c",  | |
|               | |                  |   "cluster_id": "74017671-f945-4843-859f-05492195b8d7",       | |
|               | |                  |   "cluster_node_index": "1"                                   | |
|               | |                  | }                                                             | |
|               | | name             | node_1                                                        | |
|               | | progress         | 0                                                             | |
|               | | security_groups  | default                                                       | |
|               | | status           | ACTIVE                                                        | |
|               | | volumes_attached | []                                                            | |
|               | +------------------+---------------------------------------------------------------+ |
| id            | f3234289-388d-430b-93ce-b0ec5aec5a0c                                                 |
| index         | 1                                                                                    |
| init_at       | 2017-05-27T02:20:44Z                                                                 |
| location      | -                                                                                    |
| metadata      | {                                                                                    |
|               |   "key": "value"                                                                     |
|               | }                                                                                    |
| name          | node_1                                                                               |
| physical_id   | 3b5c414a-a20c-47eb-86ea-ef5ee5a89fa7                                                 |
| profile_id    | e5884817-3f08-4468-acd7-a8da836a4be4                                                 |
| profile_name  | cirros                                                                               |
| project_id    | 68330cdfaf204bc7961405e8d1e62e5d                                                     |
| role          | -                                                                                    |
| status        | ACTIVE                                                                               |
| status_reason | Creation succeeded                                                                   |
| updated_at    | -                                                                                    |
| user_id       | 1f561a7e171641a9a5e8e8b5f92a2b1e                                                     |
+---------------+--------------------------------------------------------------------------------------+
```

## 列出节点

使用`senlin node-list`命令可以列出已经存在的节点列表。附加`-c`参数指定伸缩组只列出某一伸缩组下的节点列表，`-f`参数根据给定条件筛选，`-o`参数给定条件对node进行筛选，`-l`参数表示最多列出几个节点，`-F`参数显示完整的id。

```
$ senlin node-list
+----------+--------+-------+--------+------------+-------------+--------------+----------------------+------------+
| id       | name   | index | status | cluster_id | physical_id | profile_name | created_at           | updated_at |
+----------+--------+-------+--------+------------+-------------+--------------+----------------------+------------+
| f3234289 | node_1 | 1     | ACTIVE | 74017671   | 3b5c414a    | cirros       | 2017-05-27T02:23:12Z | -          |
+----------+--------+-------+--------+------------+-------------+--------------+----------------------+------------+

```

## 更新节点

使用`senlin node-update`命令更新节点的相关信息。附加`-n`参数修改节点名称，`-p`参数修改节点的启动配置，节点会根据启动配置做recreate操作，`-M`添加元数据。

```
$ senlin node-update node_1 -n node_2
+---------------+--------------------------------------+
| Property      | Value                                |
+---------------+--------------------------------------+
| cluster_id    | 74017671-f945-4843-859f-05492195b8d7 |
| created_at    | 2017-05-27T02:23:12Z                 |
| data          | {}                                   |
| dependents    | {}                                   |
| details       | -                                    |
| id            | f3234289-388d-430b-93ce-b0ec5aec5a0c |
| index         | 1                                    |
| init_at       | 2017-05-27T02:20:44Z                 |
| location      | -                                    |
| metadata      | {                                    |
|               |   "key": "value"                     |
|               | }                                    |
| name          | node_1                               |
| physical_id   | 3b5c414a-a20c-47eb-86ea-ef5ee5a89fa7 |
| profile_id    | e5884817-3f08-4468-acd7-a8da836a4be4 |
| profile_name  | cirros                               |
| project_id    | 68330cdfaf204bc7961405e8d1e62e5d     |
| role          | -                                    |
| status        | ACTIVE                               |
| status_reason | Creation succeeded                   |
| updated_at    | -                                    |
| user_id       | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+---------------+--------------------------------------+
```

## 删除节点

使用`senlin node-delete`命令可以删除指定节点。

```
$ senlin node-delete node_1
node_1: accepted by action 5e5f4dc7-2353-437e-a8f5-96e7df5ef806
```

## 移除节点

使用`senlin node-remove`命令可以将节点从senlin移除变为普通云主机，由nova管理。

```
$ senlin node-remove node_1
Request accepted
```

执行命令后，节点会变为普通云主机。通过`node-list`命令查询发现node_1已经不存在，但是仍然可以在nova中看到。

## 设置节点保护

将节点设置为保护状态后，节点不会被伸缩组的scale-in和健康检查操作删除，需要移除保护后才可以删除。

#### 设置保护状态

```
$ senlin node-protect-set node_1
Request accepted
```

然后执行`senlin node-show node_1`命令可以看到node_1状态变为`PROTECTED`


#### 移除保护状态

```
$ senlin node-protect-remove node_1
Request accepted
```

然后执行`senlin node-show node_1`命令可以看到node_1状态变为`ACTIVE`

## 检测节点状态

用`senlin node-check`命令重新检查节点并更新节点状态。

```
$ senlin node-check node_1
Request accepted
```

将node对应的云主机关机后执行`node-check`命令，node会变为`ERROR`状态，再将云主机开机，然后执行`node-check`命令，node会变为`ACTIVE`状态。

## 恢复节点

使用`senlin node-recover`命令恢复节点，执行recreate操作。

```
$ senlin node-recover node_1
Request accepted
```

如果一个节点处于`ERROR`状态，可以执行`node-recover`命令恢复节点，对应的nova操作是`recreate`,再次查询可以发现云主机的id和网络等信息都已经改变。
