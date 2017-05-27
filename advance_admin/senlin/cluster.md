# 伸缩组（集群）

集群是一些相同资源的集合，弹性伸缩就是增加或者减少集群里资源的数量来实现。可以给集群附加相关的策略，实现集群的自动伸缩。

## 创建集群

创建集群的命令为`senlin cluster-create`，创建时必须指定名称和启动配置。部分参数及其意义如下：

- -p 启动配置
- -n 集群中最小节点数量
- -m 集群中最大节点数量
- -c 创建集群时集群中节点数量
- -t 创建集群的超时时间
- -M 添加元数据

```
$ senlin cluster-create -p cirros -n 1 -m 3 -c 1 cluster_1
+------------------+--------------------------------------+
| Property         | Value                                |
+------------------+--------------------------------------+
| created_at       | -                                    |
| data             | {}                                   |
| dependents       | {}                                   |
| desired_capacity | 1                                    |
| domain_id        | -                                    |
| id               | e4eea354-5534-4fd7-8e75-330c70b80fc3 |
| init_at          | 2017-05-27T02:52:05Z                 |
| is_profile_only  | -                                    |
| location         | -                                    |
| max_size         | 3                                    |
| metadata         | {}                                   |
| min_size         | 1                                    |
| name             | cluster_1                            |
| node_ids         |                                      |
| profile_id       | e5884817-3f08-4468-acd7-a8da836a4be4 |
| profile_name     | cirros                               |
| project_id       | 68330cdfaf204bc7961405e8d1e62e5d     |
| status           | INIT                                 |
| status_reason    | Initializing                         |
| timeout          | 3600                                 |
| updated_at       | -                                    |
| user_id          | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+------------------+--------------------------------------+
```

## 查看集群

使用`senlin cluster-show`命令查看集群的信息。

```
$ senlin cluster-show cluster_1
+------------------+---------------------------------------------------------------+
| Property         | Value                                                         |
+------------------+---------------------------------------------------------------+
| created_at       | 2017-05-27T02:53:43Z                                          |
| data             | {}                                                            |
| dependents       | {}                                                            |
| desired_capacity | 1                                                             |
| domain_id        | -                                                             |
| id               | e4eea354-5534-4fd7-8e75-330c70b80fc3                          |
| init_at          | 2017-05-27T02:52:05Z                                          |
| is_profile_only  | -                                                             |
| location         | -                                                             |
| max_size         | 3                                                             |
| metadata         | {}                                                            |
| min_size         | 1                                                             |
| name             | cluster_1                                                     |
| node_ids         | d1d52e80-778a-4152-923f-25c4f6509245                          |
| profile_id       | e5884817-3f08-4468-acd7-a8da836a4be4                          |
| profile_name     | cirros                                                        |
| project_id       | 68330cdfaf204bc7961405e8d1e62e5d                              |
| status           | ACTIVE                                                        |
| status_reason    | create: number of active nodes is above desired_capacity (1). |
| timeout          | 3600                                                          |
| updated_at       | -                                                             |
| user_id          | 1f561a7e171641a9a5e8e8b5f92a2b1e                              |
+------------------+---------------------------------------------------------------+
```

## 列出集群

使用`senlin cluster-list`命令列出已经创建的集群列表。附加`-f`参数进行筛选，`-o`参数排序，`-F`参数显示完整id。

```
$ senlin cluster-list
+----------+-----------+--------+----------------------+------------+
| id       | name      | status | created_at           | updated_at |
+----------+-----------+--------+----------------------+------------+
| e4eea354 | cluster_1 | ACTIVE | 2017-05-27T02:53:43Z | -          |
+----------+-----------+--------+----------------------+------------+
```

## 更新集群

使用`senlin cluster-update`命令更新集群的相关信息。

- -p 指定新的启动配置，集群会根据新的启动配置做recreate操作。
- -P 是否只更新启动配置，不做recreate操作。只有新增加的节点会根据新的启动配置来创建。
- -n 新的名称
- -M 添加新的元数据
- -t 更新操作的超时时间

```
$ senlin cluster-update cluster_1 -n cluster_2 -p cirros_2 -P true
+------------------+---------------------------------------------------------------+
| Property         | Value                                                         |
+------------------+---------------------------------------------------------------+
| created_at       | 2017-05-27T02:53:43Z                                          |
| data             | {}                                                            |
| dependents       | {}                                                            |
| desired_capacity | 1                                                             |
| domain_id        | -                                                             |
| id               | e4eea354-5534-4fd7-8e75-330c70b80fc3                          |
| init_at          | 2017-05-27T02:52:05Z                                          |
| is_profile_only  | -                                                             |
| location         | -                                                             |
| max_size         | 3                                                             |
| metadata         | {}                                                            |
| min_size         | 1                                                             |
| name             | cluster_1                                                     |
| node_ids         | d1d52e80-778a-4152-923f-25c4f6509245                          |
| profile_id       | e5884817-3f08-4468-acd7-a8da836a4be4                          |
| profile_name     | cirros                                                        |
| project_id       | 68330cdfaf204bc7961405e8d1e62e5d                              |
| status           | ACTIVE                                                        |
| status_reason    | create: number of active nodes is above desired_capacity (1). |
| timeout          | 3600                                                          |
| updated_at       | -                                                             |
| user_id          | 1f561a7e171641a9a5e8e8b5f92a2b1e                              |
+------------------+---------------------------------------------------------------+
```

更新集群后使用`cluster-show`命令再次查看集群信息，会发现相关信息已经改变。

## 删除集群

使用`senlin cluster-delete`命令删集群。

```
$ senlin cluster-delete cluster_1
cluster_1: accepted by action 3af143d2-cb17-4972-984c-4d2b9143dea6
```

## 检查集群状态

使用`senlin cluster-check`命令重新检查并更新集群的状态。

```
$ senlin cluster-check cluster_1
Cluster check request on cluster cluster_1 is accepted by action 510f5f3c-f4ef-463c-9460-d7d192cbb88e.
```

将集群中一个节点对应的云主机关机，再执行`cluster-check`命令，集群会变为`WARNING`状态。将云主机重启后再执行`cluster-check`命令，集群会变为`ACTIVE`状态。

## 暂停/启用集群

讲集群设置为暂停状态后，会停止集群的伸缩活动，集群不能够自动向伸缩组添加或者删除节点。

暂停状态的集群不会自动伸缩，即不会响应scale-out和scale-in操作，然是仍然可以手动向集群中增加或者删除节点。

#### 暂停集群

使用`senlin cluster-suspend`命令暂停集群。

```
$ senlin cluster-suspend cluster_1
Cluster suspend request on cluster cluster_1 is accepted by action 20f7a7cf-2132-47ea-8361-d72997e3c822.
```

通过命令`cluster-show`查看集群变为`SUSPEND`状态，手动执行`cluster-out`命令会提示集群处于暂停状态。

#### 启用集群

使用`senlin cluster-resume`命令重新恢复集群。

```
$ senlin cluster-resume cluster_1
Cluster resume request on cluster cluster_1 is accepted by action 284d5c8b-f016-4c31-8b1b-b270cffa7a39.
```

## 恢复集群

使用`senlin cluster-recover`命令恢复节点，执行recreate操作。

```
$ senlin cluster-recover cluster_1
Cluster recover request on cluster cluster_1 is accepted by action c4462b14-c44a-4824-8b7e-464e0824c739.
```

当集群处于异常状态时，`cluster-recover`命令会恢复集群，对集群内所有节点做recover操作，对应的云主机做recreate操作。将会改变云主机id和网络等信息。

## 改变集群大小

使用`senlin cluster-resize`命令可以修改集群大小。

- -c 修改集群内节点为指定数量
- -n 集群内节点最小容量
- -m 集群内节点最大容量

```
$ senlin cluster-resize -c 2 cluster_1
Request accepted by action: 2180ad8e-99d8-468e-bbce-56ea29710a7e
```

然后执行`cluster-show`命令可以看到集群内云主机数量已经改变。不论指定的数量为多少，集群大小都在集群的容量之内。

## 集群伸缩操作

集群伸缩操作即向集群中增加或者减少几个节点。

使用`senlin cluster-scale-out`命令进行伸或者使用`senlin cluster-scale-in`命令进行缩。`-c`参数指定改变的大小。

```
senlin cluster-scale-out -c 1 cluster
Request accepted by action 29b0deff-deb5-4d4b-8b81-e529f16f433e
```

再次执行`cluster-show`命令可以看到集群内节点数量已经增加或者减少。

## 管理集群中的节点

使用`senlin cluster-node-list`列出当前集群中的节点。

- -f 根据条件筛选
- -l 最多显示几条结果
- -F 显示完整id

```
$ senlin cluster-node-list -F cluster_1
+--------------------------------------+-----------------+-------+--------+--------------------------------------+----------------------+
| id                                   | name            | index | status | physical_id                          | created_at           |
+--------------------------------------+-----------------+-------+--------+--------------------------------------+----------------------+
| a604cd19-c380-465d-b776-2117d32716d8 | as-4741cacc-001 | 1     | ACTIVE | 9962b75c-b584-4b4f-bc1a-e139e09570fb | 2017-05-27T03:15:30Z |
| 59f4efb5-18c8-4c4f-8c6b-836ebe72458a | as-4741cacc-003 | 3     | ACTIVE | d4853822-6f47-45e2-a59f-4dd005ab6097 | 2017-05-27T03:36:54Z |
+--------------------------------------+-----------------+-------+--------+--------------------------------------+----------------------+
```

使用`senlin cluster-node-add`命令将已有节点加入到集群中，或者使用`senlin cluster-node-del`讲集群中的节点移出集群但不删除。`-n`参数指定节点。

```
senlin cluster-node-add -n as-4741cacc-003 cluster_1
Request accepted by action: 5f85b806-f719-4a5d-a58c-f5281714554b
```

再次执行`cluster-node-list`命令发现要移除的节点已经不在当前集群，但是通过`node-list`命令仍然可以查看得到。

## 集群的策略

使用`senlin cluster-policy-attach`命令附加策略到集群，使用`senlin cluster-policy-detach`命令解除集群和策略的绑定。使用`-p`参数指定策略，`-e`参数指定策略是否生效。

```
$ senlin cluster-policy-attach -p scale-out cluster_1
Request accepted by action: 87c3eb7d-db5a-41f6-993d-59860d6bb18b
```

将策略附加到集群之后，使用`senlin cluster-policy-list`命令列出附加在集群上的策略。

- -f 根据条件筛选
- -o 排序
- -F 显示完整id

```
$ senlin cluster-policy-list cluster_1
+-----------+-------------+---------------------------+------------+
| policy_id | policy_name | policy_type               | is_enabled |
+-----------+-------------+---------------------------+------------+
| 1d3509d0  | scale-out   | senlin.policy.scaling-1.0 | True       |
| c9f99f8f  | scale-in    | senlin.policy.scaling-1.0 | True       |
+-----------+-------------+---------------------------+------------+
```

使用`senlin cluster-policy-update`命令更新附加在集群上的策略的状态，`-p`参数指定要更新的策略，`-e`参数指定是否启用。

```
$ senlin cluster-policy-update -p scaling -e false cluster_1
Request accepted by action: 87c3eb7d-db5a-41f6-993d-59860d6bb18b
```

然后使用`cluster-policy-list`命令可以看到这个策略已经呗停用了。

使用`senlin cluster-policy-show`命令查看附加在集群上的策略的信息。`-p`参数指定策略。

```
$ senlin cluster-policy-show -p scaling cluster_1
Request accepted by action 03a8c9be-22e3-4ee6-99ee-4434dd437f78
```
