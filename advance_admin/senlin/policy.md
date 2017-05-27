# 关联策略

策略定义了一些规则，表示在特定事件发生时应该遵循怎样的规则去执行一些动作。策略与集群配合使用，将策略添加到集群，相应的策略根据策略内容完成集群的改变。

## Policy类型

使用`senlin policy-type-list`命令查看支持的策略类型

```
$ senlin policy-type-list
+------------------------------------+
| name                               |
+------------------------------------+
| senlin.policy.affinity-1.0         |
| senlin.policy.deletion-1.0         |
| senlin.policy.eayun_lbaas-1.0      |
| senlin.policy.health-1.0           |
| senlin.policy.loadbalance-1.0      |
| senlin.policy.region_placement-1.0 |
| senlin.policy.scaling-1.0          |
| senlin.policy.zone_placement-1.0   |
+------------------------------------+
```

*使用senlin.policy.eayun_lbaas-1.0代替senlin.policy.loadbalance-1.0策略*

## 创建policy

假设在当前路径下已经编写了一个名为'scaling_out_policy.yaml'的policy，使用`senlin policy-create`命令创建一个名为scale-out的policy：

```
$ senlin policy-create -s scaling_out_policy.yaml scale-out
+------------+--------------------------------------+
| Property   | Value                                |
+------------+--------------------------------------+
| created_at | 2017-05-27T02:04:08Z                 |
| data       | {}                                   |
| id         | edbc50b4-5f9c-49ee-8ee1-60ad444ae967 |
| location   | -                                    |
| name       | scale-out                            |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d     |
| spec       | {                                    |
|            |   "version": 1.0,                    |
|            |   "type": "senlin.policy.scaling",   |
|            |   "properties": {                    |
|            |     "adjustment": {                  |
|            |       "cooldown": 60,                |
|            |       "type": "CHANGE_IN_CAPACITY",  |
|            |       "number": 1                    |
|            |     },                               |
|            |     "event": "CLUSTER_SCALE_OUT"     |
|            |   }                                  |
|            | }                                    |
| type       | senlin.policy.scaling-1.0            |
| updated_at | -                                    |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+------------+--------------------------------------+
```

每种类型的policy编写方法如下。

### 负载均衡策略

负载均衡策略的作用是自动将伸缩组下的节点添加到负载均衡中作为pool下的member成员。

使用`senlin policy-type-show senlin.policy.eayun_lbaas-1.0`命令可以查看负载均衡策略的详细信息，参照返回的信息创建新的策略。

部分参数及其意义如下：

- pool_id: 已存在负载均衡的pool id
- protocol_port: pool下member成员的服务端口
- lb_status_timeout: 请求 loadbance 超时时间

下面是一个简单的示例：

```
type: senlin.policy.eayun_lbaas
version: 1.0
properties:
  pool:
    pool_id: 7ccbcb66-5eab-4885-af51-dfd17f85647b
    protocol_port: 80
  lb_status_timeout: 300
```

### 健康检查策略

健康检查策略的作用是自定检查伸缩组下云主机的健康状态，当检测到云主机异常时，自动替换不健康云主机。

使用`senlin policy-type-show senlin.policy.health-1.0`命令可以查看负载均衡策略的详细信息，参照返回的信息创建新的策略。

部分参数及其意义如下：

- type: 健康检查的类型，包括
  - NODE_STATUS_POLLING 检测节点是否处于 inactive 状态，按时间周期检测
  - VM_LIFFCYCLE_EVENTS 事件通告，通告获取事件信息来进行节点状态判定(目前不支持)
- interval 周期检测时间
- recovery action 恢复方式，支持的方式包括
  - RECREATE 重建(删除后，创建新的)
  - REBUILD 重建(调用 nova 直接重新构建，目前不支持)

下面是一个简单的示例：

```
type: senlin.policy.health
version: 1.0
properties:
  detection:
    type: NODE_STATUS_POLLING
    options:
      interval: 600
  recovery:
    actions:
      - RECREATE
```

### 删除策略

删除策略的作用是当伸缩组缩小时，决定删除最老的，删除最新的，还是随机删除伸缩组下的云主机

使用`senlin policy-type-show senlin.policy.deletion-1.0`命令可以查看负载均衡策略的详细信息，参照返回的信息创建新的策略。

部分参数及其意义如下：

- criteria: 删除的规则，可选OLDEST_FIRST，YOUNGEST_FIRST，RANDOM
- destroy_after_deletion: 从伸缩组移除后是否在nova删除云主机
- grace_period: 实际删除云主机前的等待时间

下面是一个简单的示例：

```
type: senlin.policy.deletion
version: 1.0
properties:
  criteria: OLDEST_FIRST
  destroy_after_deletion: True
  grace_period: 0
```

### 伸缩策略

伸缩策略可以定义与云主机伸缩的相关规则

使用`senlin policy-type-show senlin.policy.scaling-1.0`命令可以查看负载均衡策略的详细信息，参照返回的信息创建新的策略。

部分参数及其意义如下：

- event: 与策略相关联的事件，包括
  - CLUSTER_SCALE_OUT： 集群扩展时
  - CLUSTER_SCALE_IN： 集群收缩时
- type: 伸缩类型，包括
  - EXACT_CAPACITY： 调整当前节点数到指定数量
  - CHANGE_IN_CAPACI： 调整几台数量
  - CHANGE_IN_PERCENTAGE： 调整数量在集群内的百分比
- number: 调整的数量或百分百
- cooldown: 再次执行伸缩活动的冷却事件

下面是一个简单的示例：

```
type: senlin.policy.scaling
version: 1.0
properties:
  event: CLUSTER_SCALE_OUT
  adjustment:
    type: CHANGE_IN_CAPACITY
    number: 1
    cooldown: 60
```

## 查看policy

使用`senlin policy-list`命令可以查看已经创建的profile的列表，
然后使用`senlin profile-show`命令可以查看已经创建的profile的详细信息，例如：

```
$ senlin policy-show scale-out
+------------+--------------------------------------+
| Property   | Value                                |
+------------+--------------------------------------+
| created_at | 2017-05-27T02:04:08Z                 |
| data       | {}                                   |
| id         | edbc50b4-5f9c-49ee-8ee1-60ad444ae967 |
| location   | -                                    |
| name       | scale-out                            |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d     |
| spec       | {                                    |
|            |   "version": 1.0,                    |
|            |   "type": "senlin.policy.scaling",   |
|            |   "properties": {                    |
|            |     "adjustment": {                  |
|            |       "cooldown": 60,                |
|            |       "type": "CHANGE_IN_CAPACITY",  |
|            |       "number": 1                    |
|            |     },                               |
|            |     "event": "CLUSTER_SCALE_OUT"     |
|            |   }                                  |
|            | }                                    |
| type       | senlin.policy.scaling-1.0            |
| updated_at | -                                    |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+------------+--------------------------------------+
```

## 更新policy

使用`senlin policy-update`命令可以更新策略相关属性，目前只支持修改策略名称，例如：

```
$ senlin policy-update scale-out -n new_policy
+------------+--------------------------------------+
| Property   | Value                                |
+------------+--------------------------------------+
| created_at | 2017-05-27T02:04:08Z                 |
| data       | {}                                   |
| id         | edbc50b4-5f9c-49ee-8ee1-60ad444ae967 |
| location   | -                                    |
| name       | new_policy                           |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d     |
| spec       | {                                    |
|            |   "version": 1.0,                    |
|            |   "type": "senlin.policy.scaling",   |
|            |   "properties": {                    |
|            |     "adjustment": {                  |
|            |       "cooldown": 60,                |
|            |       "type": "CHANGE_IN_CAPACITY",  |
|            |       "number": 1                    |
|            |     },                               |
|            |     "event": "CLUSTER_SCALE_OUT"     |
|            |   }                                  |
|            | }                                    |
| type       | senlin.policy.scaling-1.0            |
| updated_at | 2017-05-27T02:11:11Z                 |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+------------+--------------------------------------+
```

## 删除policy

使用`senlin policy-delete`命令可以删除指定的策略，但是只限于删除未与伸缩组关联的policy,例如：

```
$ senlin policy-delete scale-out
Policy deleted: ['scale-out']
```
