# 云数据库

云数据库利用已有的 EayunStack 基础服务资源提供数据库服务，目前支持的数据库服务有：

* MySQL

## 客户端使用

云数据库服务使用 Docker container 部署在所有控制节点服务器，因此在控制节点基础系统中没有云数据库服务的客户端。

云数据库服务在每台控制节点服务器上都分别运行 3 个 Docker container：

> 注：
> 下面表格中 `<fuel_node_ip>` 指的是环境中 Fuel 节点的 PXE 网络的 IP 地址。

| 云数据库底层基础服务 | Docker container | Docker image |
|----|------------|----|
| trove\_api | Trove API | `<fuel_node_ip>:5010/eayunstack/trove-api` |
| trove\_taskmanager | Trove TaskManager | `<fuel_node_ip>:5010/eayunstack/trove-taskmanager` |
| trove\_conductor | Trove Conductor | `<fuel_node_ip>:5010/eayunstack/trove-conductor` |

要使用 Trove 客户端，可以在任一控制节点以上述 3 个 Docker image 启动一个 container ，如：

```bash
  # docker run -it --name=trove_client -u root <fuel_node_ip>:5010/eayunstack/trove-api /bin/bash
```

执行上述命令后，会启动一个新的 container ，可以用于客户端使用。

在新的 container 中使用客户端时，需要自行创建 openrc 或者相应环境变量。

客户端使用完毕之后，直接执行

```base
  # exit
```

会退出并关闭 container ，该 container 可以一直保留以备下次使用。下次使用时，执行：

> 注：
> 需要在之前创建该 container 的控制节点执行

```bash
  # docker start -ai trove_client
```

即可重新启动并进入客户端 container 。

如果确定不会再使用该 container ，应进行删除：

> 注：
> 需要在之前创建该 container 的控制节点执行

```bash
  # docker rm trove_client
```
