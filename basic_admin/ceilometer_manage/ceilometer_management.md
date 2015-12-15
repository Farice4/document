# ceilometer使用 #

ceilometer功能是对平台各类数据进行收集，为平台计费和监控提供数据支撑。

ceilometer中主要的概念以下：

* resource：是被监控的资源对象。例如某一台虚拟机、某个镜像、某个卷等；

* meter：计量项。例如一台虚拟机，有运行时间、cpu使用率、内存使用率等；

* sample：采样数据，是每个采集点上的meter对应的值；

* statistics：统计数据，某个周期内的某项计量项的统计数据，如平均值、峰值等；

* alarm：报警，一个报警包含报警条件与报警操作，alarm分为两类：

    > * alarm-threshold：阈值报警

    > * alarm-combination：组合报警

其中的关系是：

* 每个resource都会有一些meter；

* 平台会根据配置的采集周期，按时去采集某个时间点某个resource的某个meter的值，得到sample值；

* 平台根据多个时间点的sample，来进行数据统计，计算出某段时间meter值的max、min等，得到statistics统计值；

* 一个alarm根据statistics对报警条件进行判断报警状态，然后进行响应的操作；

本节主要内容有监控资源的查看、计量项的查询、计量项采样的查询、计量项的统计查询、警报的查询、创建和删除。

> ###### 注意
> ceilometer操作大多是在命令行，web界面只有一个简单的展示页面。


