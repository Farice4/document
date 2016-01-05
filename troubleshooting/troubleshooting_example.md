# 常见故障排除方法示例

本节主要介绍在运维过程中, 发生故障时, 如果迅速的定位故障来源, 以几个小例子来说明:

* [ 示例1 ](#1)
* [ 示例2 ](#2)

##示例1
![trouble-example1](../pictures/trouble-example1.jpg)
 
如图, 创建云主机失败是较常见的故障, 一般界面上会提示一些相关信息，比如找不到主机等，但是大部分情况界面提示的信息并能够让我们得到足够的信息，真实的原因有可能是限额满了, 服务不可用了, 发生 bug 了, 当发生故障时, 运维人员需要会排查日志来检测异常:

* 用运维工具检测系统集群环境, 仔细查看上面的 **WARNING** 和 **ERROR**信息
* 如果运维工具检测系统正常, 那么需要进一步的排查日志, 先准备好相关的信息, 比如云主机的ID, 发生错误的大概时间等, 利用这些信息去检查故障根源
* 由于创建云主机对应的是 nova 服务, 需要去查看 nova 的相关log, 更多 nova log 内容请参考 TODO, 这里我们去查看 nova-api.log nova-conductor.log nova-scheduler.log 文件, 找到发生 ERROR 的相关日志信息 , 看是不是最近时间的, 比如我创建这个云主机是 12:42 左右, 在 nova-compute.log 里面正好搜索到一个12:43 的日志信息, 再根据进一步的信息,
```
2015-05-22 12:43:01.844 46552 ERROR nova.compute.manager [req-244527c1-6011-4ab1-8ec2-16cf64a0106e None] [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5] Instance failed to spawn
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5] Traceback (most recent call last):
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/compute/manager.py", line 2231, in _build_resources
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     yield resources
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/compute/manager.py", line 2101, in _build_and_run_instance
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     block_device_info=block_device_info)
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/driver.py", line 2622, in spawn
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     write_to_disk=True)
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/driver.py", line 4154, in _get_guest_xml
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     xml = conf.to_xml()
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/config.py", line 81, in to_xml
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     root = self.format_dom()
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/config.py", line 1683, in format_dom
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     self._format_basic_props(root)
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/config.py", line 1622, in _format_basic_props
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     metadata.append(m.format_dom())
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/config.py", line 1912, in format_dom
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     meta.append(self._text_node("name", self.name))
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]   File "/usr/lib/python2.7/site-packages/nova/virt/libvirt/config.py", line 65, in _text_node
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]     child.text = str(value)
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5] UnicodeEncodeError: 'ascii' codec can't encode characters in position 0-1: ordinal not in range(128)
2015-05-22 12:43:01.844 46552 TRACE nova.compute.manager [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5]
2015-05-22 12:43:01.847 46552 AUDIT nova.compute.manager [req-244527c1-6011-4ab1-8ec2-16cf64a0106e None] [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5] Terminating instance
2015-05-22 12:43:01.859 46552 WARNING nova.virt.libvirt.driver [-] [instance: 1e2bd824-4e4e-44de-9b74-e30944a6edf5] During wait destroy, instance disappeared.
```
到这里已经很明显了, 创建虚拟机的时候， 在 libvirt 启动云主机的时候发生了异常，有了这段信息，就可以非常快速准确的解决此问题。

> ###### 注意
> 上面的异常其实是一个 openstack 的 bug，使用中文名创建云主机时会出错，已经开发阶段修复了此 bug，这里只是作为一个例子
 
> #### 技巧
> 重现问题查看 log 的时候，只需要截取创建新的云主机这段期间的 log，比如对于计算节点的 log，在点击创建之前 tailf /var/log/nova/nova-compute.log > /tmp/compute.log, 任务结束后查看该log更加清晰。
 
> #### 技巧
> 对于多个主机的 log 查看，由于是分布式系统，推荐使用 multitail 工具，比如查看 nova-api 工具, 下述命令会将三台controller上的 nova-api.log 保存到本地的 api.log.
> 在创建云主机前执行改命令，创建完之后按 q 取消即可。
>```
># multitail -A api.log -L "ssh controller1 tailf /var/log/nova/nova-api.log" -L "ssh controller2 tailf /var/log/nova/nova-api.log" -L "ssh controller3 tailf /var/log/nova/nova-api.log"
>```

##示例2
如图:
