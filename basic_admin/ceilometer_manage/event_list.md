# 查看事件信息
　　EayunStack各服务在运行过程中，会往notifications消息队列发送通知，通知通常是描述一个资源上发生的事情．通知可以被生成事件由event配置文件指定（event_definitions.yaml)

　　事件中存在事件类型，可以通过event-type-list查询事件类型，事件类型中记录了所有事件的type类型列表


### 查看事件类型

>事件类型查看使用：

    usage: ceilometer event-type-list

* example

```
# ceilometer event-type-list
+-------------------------------------------------+
| Event Type                                      |
+-------------------------------------------------+
| compute.instance.create.end                     |
| compute.instance.create.start                   |
| compute.instance.delete.end                     |
| compute.instance.delete.start                   |
| compute.instance.exists                         |
| compute.instance.finish_resize.end              |
| compute.instance.finish_resize.start            |
| compute.instance.live_migration._post.end       |
| compute.instance.live_migration._post.start     |
| compute.instance.live_migration._rollback.end   |
| compute.instance.live_migration._rollback.start |
| compute.instance.live_migration.post.dest.end   |
| compute.instance.live_migration.post.dest.start |
| compute.instance.live_migration.pre.end         |
| compute.instance.live_migration.pre.start       |
| compute.instance.power_off.end                  |
| compute.instance.power_off.start                |
| compute.instance.power_on.end                   |
| compute.instance.power_on.start                 |
| compute.instance.rebuild.end                    |
+-------------------------------------------------+
```
事件类型信息包括(compute,network,image,cinder)等类型事件信息

### 查看事件信息

使用`ceilometer event-list`查询事件，事件支持-q 筛选查询，常用的筛选查询条件如下：

|属性|备注|
|---|---|
|message_id|消息ID|
|event_type|事件类型|
|generated|产生的时间撮|

* 根据事件类型查询

```
#  ceilometer event-list -q "event_type=port.delete.end"
+--------------------------------------+-----------------+----------------------------+--------------------------------------------------------------------+
| fffd643a-4c01-49f8-92fc-05b76e1647b2 | port.delete.end | 2015-11-24T23:00:34.557000 | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | |    name    |  type  |                  value                   | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | | tenant_id  | string |     7b6ca46252b44ac88c75fc40d527f911     | |
|                                      |                 |                            | |  service   | string |         network.node-6.eayun.com         | |
|                                      |                 |                            | | request_id | string | req-155fefed-0333-4664-b65c-8cdd7f1243cf | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
| fffe0cfb-5585-4294-b770-20e1e9ccb78a | port.delete.end | 2015-11-24T15:30:16.851000 | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | |    name    |  type  |                  value                   | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | | tenant_id  | string |     26b82586066f4e1ab6ba0b0a5abc68ff     | |
|                                      |                 |                            | |  service   | string |         network.node-8.eayun.com         | |
|                                      |                 |                            | | request_id | string | req-b0da9013-ce1f-4cd1-8d1c-0ecfbf11537d | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
+--------------------------------------+-----------------+----------------------------+--------------------------------------------------------------------+
```
* 根据时间查询

```
# ceilometer event-list -q "start_time>2015-11-25T06:20:00;end_time<2015-11-25T06:35:00"
+--------------------------------------+-----------------------+----------------------------+--------------------------------------------------+
| ed749efb-eef4-4179-8de1-1e5e45ab33fb | identity.authenticate | 2015-11-25T06:27:08.539000 | +---------+--------+---------------------------+ |
|                                      |                       |                            | |   name  |  type  |           value           | |
|                                      |                       |                            | +---------+--------+---------------------------+ |
|                                      |                       |                            | | service | string | identity.node-8.eayun.com | |
|                                      |                       |                            | +---------+--------+---------------------------+ |
| f2100369-cadc-42ae-b275-addd28f12bf1 | identity.authenticate | 2015-11-25T06:20:06.647000 | +---------+--------+---------------------------+ |
|                                      |                       |                            | |   name  |  type  |           value           | |
|                                      |                       |                            | +---------+--------+---------------------------+ |
|                                      |                       |                            | | service | string | identity.node-5.eayun.com | |
|                                      |                       |                            | +---------+--------+---------------------------+ |
+--------------------------------------+-----------------------+----------------------------+--------------------------------------------------+
```

* 根据message_id查询

```
# ceilometer event-list -q "message_id=25cd4620-03cf-43cf-b59e-4a8717eb221b"
+--------------------------------------+-----------------+----------------------------+--------------------------------------------------------------------+
| Message ID                           | Event Type      | Generated                  | Traits                                                             |
+--------------------------------------+-----------------+----------------------------+--------------------------------------------------------------------+
| 25cd4620-03cf-43cf-b59e-4a8717eb221b | port.delete.end | 2015-11-16T07:15:04.983000 | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | |    name    |  type  |                  value                   | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
|                                      |                 |                            | | tenant_id  | string |     2d3195523f004b45a8d1b00021677250     | |
|                                      |                 |                            | |  service   | string |         network.node-8.eayun.com         | |
|                                      |                 |                            | | request_id | string | req-410ac8e6-a516-4927-81b8-d0b71fccf26f | |
|                                      |                 |                            | +------------+--------+------------------------------------------+ |
+--------------------------------------+-----------------+----------------------------+--------------------------------------------------------------------+
```
```
