# 查看资源的详情 #

资源对象的详细信息，包括元数据、project_id、resource_id、source、user_id等。

## 用法 ##

`ceilometer resource-show <RESOURCE_ID>`

必选参数：

`<RESOURCE_ID>`		资源对象的id

## 示例 ##

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


