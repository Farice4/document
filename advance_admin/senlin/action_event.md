# 事件和动作

事件和动作表示在服务运行过程中产生的事件和执行的动作，可以根据这些信息查看集群运行的相信信息。

## 事件

### 列出事件

使用`senlin event-list`列出事件。

- -f 根据给定条件筛选
- -l 最多显示几条结果
- -o 根据给定条件排序
- -F 显示完整id

```
$ senlin event-list -l 5 - obj_name=test
+----------+-----------+----------+----------+----------+-----------------------+-----------+-------+------------+
| id       | timestamp | obj_type | obj_id   | obj_name | action                | status    | level | cluster_id |
+----------+-----------+----------+----------+----------+-----------------------+-----------+-------+------------+
| 159398d9 |           | CLUSTER  | 0726452d | test     | CLUSTER_CHECK         | START     | 20    |            |
| 1657d567 |           | CLUSTER  | 0726452d | test     | CLUSTER_ATTACH_POLICY | START     | 20    |            |
| 2ca355df |           | CLUSTER  | 0726452d | test     | CLUSTER_ATTACH_POLICY | SUCCEEDED | 20    |            |
| 3453bd52 |           | CLUSTER  | 0726452d | test     | CLUSTER_CREATE        | START     | 20    |            |
| 554d03f3 |           | CLUSTER  | 0726452d | test     | CLUSTER_CREATE        | SUCCEEDED | 20    |            |
+----------+-----------+----------+----------+----------+-----------------------+-----------+-------+------------+
```

### 显示事件详情

使用`senlin event-show`命令查看事件详情。

```
$ senlin event-show 159398d9
+---------------+--------------------------------------+
| Property      | Value                                |
+---------------+--------------------------------------+
| action        | CLUSTER_CHECK                        |
| cluster_id    | -                                    |
| generated_at  | 2017-05-04T03:38:55+00:00            |
| id            | 159398d9-3406-4b0a-baa0-ab6af85e4812 |
| level         | 20                                   |
| location      | -                                    |
| name          | -                                    |
| obj_id        | 0726452d-fe22-4bea-9f86-2089894caad7 |
| obj_name      | test                                 |
| obj_type      | CLUSTER                              |
| project_id    | 68330cdfaf204bc7961405e8d1e62e5d     |
| status        | START                                |
| status_reason | The action is being processed.       |
| user_id       | 1f561a7e171641a9a5e8e8b5f92a2b1e     |
+---------------+--------------------------------------+
```

## 动作

### 列出动作

使用`senlin action-list`列出事件。

- -f 根据给定条件筛选
- -l 最多显示几条结果
- -o 根据给定条件排序
- -F 显示完整id

```
$ senlin action-list -l 5 -f action=NODE_CREATE
+----------+----------------------+-------------+-----------+--------------------------------------+------------+-------------+----------------------+
| id       | name                 | action      | status    | target_id                            | depends_on | depended_by | created_at           |
+----------+----------------------+-------------+-----------+--------------------------------------+------------+-------------+----------------------+
| 17232c00 | node_create_1ce773ca | NODE_CREATE | FAILED    | 1ce773ca-64d7-4163-8b9e-fc39875c763d |            |             | 2017-05-08T06:16:31Z |
| 3a9ea2c1 | node_create_e395e10d | NODE_CREATE | FAILED    | e395e10d-400b-46fd-9eba-b63c4beedc20 |            |             | 2017-05-08T05:57:27Z |
| 517b45d3 | node_create_b3dc62af | NODE_CREATE | FAILED    | b3dc62af-0707-4ea6-9eac-09d2bbe0b491 |            |             | 2017-05-08T06:06:22Z |
| 62f81bf0 | node_create_0d4bf70f | NODE_CREATE | FAILED    | 0d4bf70f-2cd0-4774-be50-6ac17840a98f |            |             | 2017-05-08T06:03:46Z |
| ff1ca24d | node_create_e728c019 | NODE_CREATE | SUCCEEDED | e728c019-34b2-46ac-b08d-d10fff38d649 |            |             | 2017-05-04T03:34:37Z |
+----------+----------------------+-------------+-----------+--------------------------------------+------------+-------------+----------------------+
```

### 显示动作详情

使用`senlin action-show`命令查看事件详情。

```
$ senlin action-show 17232c00
+---------------+--------------------------------------+
| Property      | Value                                |
+---------------+--------------------------------------+
| action        | NODE_CREATE                          |
| cause         | RPC Request                          |
| created_at    | 2017-05-08T06:16:31Z                 |
| depended_by   |                                      |
| depends_on    |                                      |
| end_at        | 1494224256.0                         |
| id            | 17232c00-c150-43c3-b909-271aac707518 |
| inputs        | {}                                   |
| interval      | -1                                   |
| location      | -                                    |
| name          | node_create_1ce773ca                 |
| outputs       | {}                                   |
| owner_id      | -                                    |
| project_id    | -                                    |
| start_at      | 1494224128.0                         |
| status        | FAILED                               |
| status_reason | Node creation failed.                |
| target_id     | 1ce773ca-64d7-4163-8b9e-fc39875c763d |
| timeout       | 3600                                 |
| updated_at    | -                                    |
| user_id       | -                                    |
+---------------+--------------------------------------+
```
