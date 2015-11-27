# 模板资源

栈模板中的资源以一个标识的形式存在。一个资源通常代表 OpenStack 中的一个虚拟资源，如
OS::Nova::Server 这个代表虚拟机资源。

### 在线模板资源标识列表

栈模板中支持的资源列表在 heat
开发文档有一个汇总：[资源列表](http://docs.openstack.org/developer/heat/template_guide/openstack.html)

在其中可以找到每个资源标识的详细信息，但该列表很新，有一些列出的资源在我们的
环境中可能尚不支持。

### 使用命令行获取资源标识列表

每个部署了 heat 的 OpenStack 环境都可以使用命令行查询其运行中的版本
所支持的资源列表。

* 获取资源标识列表
* 获取资源标识详情
