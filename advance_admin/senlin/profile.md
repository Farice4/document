# 启动配置

启动配置定义了实例启动相关的参数，类似于heat的模板，定义了一个资源集合，不同类型的启动配置表示不同类型的资源。根据启动配置，可以创建节点和集群。

## profile类型

使用`senlin profile-type-list`命令查看支持的启动配置类型

```
$ senlin profile-type-list
+--------------------------------+
| name                           |
+--------------------------------+
| container.dockerinc.docker-1.0 |
| os.heat.stack-1.0              |
| os.nova.server-1.0             |
+--------------------------------+
```

*当前只支持基于os.nova.server-1.0的自动伸缩功能*

## 创建profile
使用`senlin profile-type-show`命令查看profile类型的详细信息，例如：`senlin profile-type-show os.nova.server-1.0`，然后参考返回的信息来创建一个新的profile。

部分参数及其代表的资源如下：

- name: 新建主机的名称
- flavor: 启动规格
- image: 镜像id或名称
- admin_pass: 登录密码
- key_name: 登录秘钥
- block_device_mapping_v2: 系统盘和数据盘
- networks: 网络、子网、端口、ip等信息
- security_groups: 所关联的安全组
- metadata: 元数据

### 编写

一个最基本的profile示例如下：

```
type: os.nova.server
version: 1.0
properties:
  flavor: 0
  image: e32bc506-a19f-4825-ac94-2faf805428c7
  networks:
    - {
        network: f0c1d489-8256-485a-8432-e282738dd8b4,
        subnet_id: 84b4031d-65ca-46f4-8434-9a4359097051
      }
```

创建一个基于卷启动的profile，并且添加数据盘：

```
type: os.nova.server
version: 1.0
properties:
  name: cirros
  flavor: 0
  block_device_mapping_v2:
    - {
        boot_index: 0,
        source_type: image,
        delete_on_termination: true,
        destination_type: volume,
        device_name: vda,
        uuid: e32bc506-a19f-4825-ac94-2faf805428c7,
        volume_size: 1
      }
    - {
        source_type: blank,
        destination_type: volume,
        device_name: vdb,
        volume_size: 1
      }
  networks:
    - {
        network: f0c1d489-8256-485a-8432-e282738dd8b4,
        subnet_id: 84b4031d-65ca-46f4-8434-9a4359097051
      }
```

*详细参照`senlin profile-type-show os.nova.server-1.0`命令返回的信息，还可以添加元数据、秘钥、安全组、多个网络等*

### 创建

假设在当前路径下已经编写了一个名为'cirros_profile.yaml'的profile，使用`senlin profile-create`命令创建一个名为cirros的profile：

```
$ senlin profile-create -s cirros_profile.yaml cirros
+------------+------------------------------------------------------------------------------+
| Property   | Value                                                                        |
+------------+------------------------------------------------------------------------------+
| created_at | 2017-05-27T01:58:42Z                                                         |
| id         | 72b66ac8-7322-4424-a6b3-a18af6569bbf                                         |
| location   | -                                                                            |
| metadata   | {}                                                                           |
| name       | cirros                                                                       |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d                                             |
| spec       | +------------+-------------------------------------------------------------+ |
|            | | property   | value                                                       | |
|            | +------------+-------------------------------------------------------------+ |
|            | | properties | {                                                           | |
|            | |            |   "flavor": 0,                                              | |
|            | |            |   "image": "e32bc506-a19f-4825-ac94-2faf805428c7",          | |
|            | |            |   "networks": [                                             | |
|            | |            |     {                                                       | |
|            | |            |       "subnet_id": "84b4031d-65ca-46f4-8434-9a4359097051",  | |
|            | |            |       "network": "f0c1d489-8256-485a-8432-e282738dd8b4"     | |
|            | |            |     }                                                       | |
|            | |            |   ]                                                         | |
|            | |            | }                                                           | |
|            | | type       | os.nova.server                                              | |
|            | | version    | 1.0                                                         | |
|            | +------------+-------------------------------------------------------------+ |
| type       | os.nova.server-1.0                                                           |
| updated_at | -                                                                            |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e                                             |
+------------+------------------------------------------------------------------------------+
```

## 查看porfile

使用`senlin profile-list`命令可以查看已经创建的profile的列表，
然后使用`senlin profile-show`命令可以查看已经创建的profile的详细信息，例如：

```
$ senlin profile-show cirros
+------------+------------------------------------------------------------------------------+
| Property   | Value                                                                        |
+------------+------------------------------------------------------------------------------+
| created_at | 2017-05-27T01:58:42Z                                                         |
| id         | 72b66ac8-7322-4424-a6b3-a18af6569bbf                                         |
| location   | -                                                                            |
| metadata   | {}                                                                           |
| name       | cirros                                                                       |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d                                             |
| spec       | +------------+-------------------------------------------------------------+ |
|            | | property   | value                                                       | |
|            | +------------+-------------------------------------------------------------+ |
|            | | properties | {                                                           | |
|            | |            |   "flavor": 0,                                              | |
|            | |            |   "image": "e32bc506-a19f-4825-ac94-2faf805428c7",          | |
|            | |            |   "networks": [                                             | |
|            | |            |     {                                                       | |
|            | |            |       "subnet_id": "84b4031d-65ca-46f4-8434-9a4359097051",  | |
|            | |            |       "network": "f0c1d489-8256-485a-8432-e282738dd8b4"     | |
|            | |            |     }                                                       | |
|            | |            |   ]                                                         | |
|            | |            | }                                                           | |
|            | | type       | os.nova.server                                              | |
|            | | version    | 1.0                                                         | |
|            | +------------+-------------------------------------------------------------+ |
| type       | os.nova.server-1.0                                                           |
| updated_at | -                                                                            |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e                                             |
+------------+------------------------------------------------------------------------------+
```

## 更新profile

使用`senlin profile-update`命令可以更新启动配置相关属性，但是只限于更新启动配置名称和增加元数据，例如：

```
$ senlin profile-update cirros -n ciros_new -M key=value
+------------+------------------------------------------------------------------------------+
| Property   | Value                                                                        |
+------------+------------------------------------------------------------------------------+
| created_at | 2017-05-27T01:58:42Z                                                         |
| id         | 72b66ac8-7322-4424-a6b3-a18af6569bbf                                         |
| location   | -                                                                            |
| metadata   | {                                                                            |
|            |   "key": "value"                                                             |
|            | }                                                                            |
| name       | cirros_new                                                                   |
| project_id | 68330cdfaf204bc7961405e8d1e62e5d                                             |
| spec       | +------------+-------------------------------------------------------------+ |
|            | | property   | value                                                       | |
|            | +------------+-------------------------------------------------------------+ |
|            | | properties | {                                                           | |
|            | |            |   "flavor": 0,                                              | |
|            | |            |   "image": "e32bc506-a19f-4825-ac94-2faf805428c7",          | |
|            | |            |   "networks": [                                             | |
|            | |            |     {                                                       | |
|            | |            |       "subnet_id": "84b4031d-65ca-46f4-8434-9a4359097051",  | |
|            | |            |       "network": "f0c1d489-8256-485a-8432-e282738dd8b4"     | |
|            | |            |     }                                                       | |
|            | |            |   ]                                                         | |
|            | |            | }                                                           | |
|            | | type       | os.nova.server                                              | |
|            | | version    | 1.0                                                         | |
|            | +------------+-------------------------------------------------------------+ |
| type       | os.nova.server-1.0                                                           |
| updated_at | 2017-05-27T02:01:08Z                                                         |
| user_id    | 1f561a7e171641a9a5e8e8b5f92a2b1e                                             |
+------------+------------------------------------------------------------------------------+
```

## 删除profile

使用`senlin profile-delete`命令可以删除指定的启动配，但是只限于删除未被使用的profile,例如：

```
$ senlin profile-delete cirros
Profile deleted: ['cirros']
```
