# 更新栈


### 通过Web horizon界面更新栈的功能不稳定，暂略

### 通过命令方式更新栈

* 更新栈，执行如下命令

> heat stack-update --template-file <template file>--environment-file <environment file> <stack name>

### 示例如下

```
# heat stack-update -f base.yaml -e base_env.yaml base_stack
+--------------------------------------+------------+--------------------+----------------------+
| id                                   | stack_name | stack_status       | creation_time        |
+--------------------------------------+------------+--------------------+----------------------+
| 194f47ee-c28b-4356-a7d3-3f74ce768bbd | base_stack | UPDATE_IN_PROGRESS | 2015-11-24T08:58:45Z |
+--------------------------------------+------------+--------------------+----------------------+
```
