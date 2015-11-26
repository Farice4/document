# 查看资源的详情 #

资源对象的详细信息，包括元数据、project_id、resource_id、source、user_id等。

## 用法 ##

`ceilometer resource-show <RESOURCE_ID>`

必选参数：

`<RESOURCE_ID>`		资源对象的id

## 示例 ##

> 虚拟机资源详情

```
# ceilometer resource-show 3708a98e-e856-4836-bc06-481d629942a4
+-------------+----------------------------------------------------------------------+
| Property    | Value                                                                |
+-------------+----------------------------------------------------------------------+
| metadata    | {u'status': u'active', u'name': u'win7', u'deleted': u'False',       |
|             | u'checksum': u'e8ef4ff6ce04820f1f73e97cd7edb918', u'created_at':     |
|             | u'2015-07-29T23:34:53', u'disk_format': u'qcow2', u'updated_at':     |
|             | u'2015-07-29T23:37:15', u'protected': u'False', u'container_format': |
|             | u'bare', u'min_disk': u'0', u'is_public': u'True', u'deleted_at':    |
|             | u'None', u'min_ram': u'0', u'size': u'7773552640'}                   |
| project_id  | 5f3c266772b94980ac2629b1c9d773c6                                     |
| resource_id | 3708a98e-e856-4836-bc06-481d629942a4                                 |
| source      | openstack                                                            |
| user_id     | None                                                                 |
+-------------+----------------------------------------------------------------------+
```

> 镜像资源详情

```
# ceilometer resource-show 02e38cc0-7f15-4dd1-9ef2-31eec4518db7
+-------------+--------------------------------------------------------------------------+
| Property    | Value                                                                    |
+-------------+--------------------------------------------------------------------------+
| metadata    | {u'properties.image_id': u'e865ceb5-5bc0-475f-b5e2-4eb8b6517d97',        |
|             | u'event_type': u'image.delete', u'container_format': u'None',            |
|             | u'min_ram': u'0', u'updated_at': u'2015-11-18T09:36:41', u'owner':       |
|             | u'578cc76cce794f15ac5b6417b9480e26', u'properties.min_disk': u'0',       |
|             | u'deleted_at': u'None', u'id': u'02e38cc0-7f15-4dd1-9ef2-31eec4518db7',  |
|             | u'size': u'0', u'disk_format': u'None', u'properties.container_format':  |
|             | u'bare', u'properties.bdm_v2': u'True', u'properties.size': u'13167616', |
|             | u'properties.disk_format': u'raw', u'deleted': u'False',                 |
|             | u'properties.block_device_mapping': u'[{"guest_format": null,            |
|             | "boot_index": 0, "no_device": null, "snapshot_id": "fd5947bd-83ec-       |
|             | 4e36-8569-c0783aabed3a", "delete_on_termination": null, "disk_bus":      |
|             | "virtio", "image_id": null, "source_type": "snapshot", "device_type":    |
|             | "disk", "volume_id": null, "destination_type": "volume", "volume_size":  |
|             | null}]', u'properties.min_ram': u'0', u'properties.checksum':            |
|             | u'64d7c1cd2b6f60c92c14662941cb7913', u'host': u'image.node-5',           |
|             | u'properties.image_name': u'apporc_test', u'min_disk': u'0',             |
|             | u'is_public': u'False', u'virtual_size': u'None', u'name': u'tttttt',    |
|             | u'checksum': u'None', u'created_at': u'2015-11-18T09:36:16',             |
|             | u'protected': u'False', u'status': u'deleted',                           |
|             | u'properties.root_device_name': u'/dev/vda'}                             |
| project_id  | 578cc76cce794f15ac5b6417b9480e26                                         |
| resource_id | 02e38cc0-7f15-4dd1-9ef2-31eec4518db7                                     |
| source      | openstack                                                                |
| user_id     | None                                                                     |
+-------------+--------------------------------------------------------------------------+
```
