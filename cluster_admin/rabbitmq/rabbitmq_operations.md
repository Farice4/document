# rabbitmqctl 命令

rabbitmq 集群的管理命令为 rabbitmqctl。

查看帮助：

    # rabbitmqctl -h

# 查看节点状态
    查看单一节点的状态：

        # rabbitmqctl status

    该命令显示当前节点 rabbitmq-server 进程 pid，版本，在运行的子程序，占用内存，侦听的端口等详细信息。

    例如：

        # rabbitmqctl status

        Status of node 'rabbit@node-1' ...
        [{pid,12641},
        {running_applications,[{rabbit,"RabbitMQ","3.3.5"},
                                {os_mon,"CPO  CXC 138 46","2.2.14"},
                                {mnesia,"MNESIA  CXC 138 12","4.11"},
                                {xmerl,"XML parser","1.3.6"},
                                {sasl,"SASL  CXC 138 11","2.3.4"},
                                {stdlib,"ERTS  CXC 138 10","1.19.4"},
                                {kernel,"ERTS  CXC 138 10","2.16.4"}]},
        {os,{unix,linux}},
        {erlang_version,"Erlang R16B03-1 (erts-5.10.4) [source] [64-bit] [smp:12:12] [async-threads:30] [hipe] [kernel-poll:true]\n"},
        {memory,[{total,96245800},
                {connection_procs,4630792},
                {queue_procs,603888},
                {plugins,0},
                {other_proc,13886272},
                {mnesia,1034704},
                {mgmt_db,0},
                {msg_index,64816},
                {other_ets,962848},
                {binary,42484128},
                {code,16695059},
                {atom,1107121},
                {other_system,14776172}]},
        {alarms,[]},
        {listeners,[{clustering,41055,"::"},{amqp,5673,"::"}]},
        {vm_memory_high_watermark,0.4},
        {vm_memory_limit,27004477440},
        {disk_free_limit,50000000},
        {disk_free,32282927104},
        {file_descriptors,[{total_limit,102300},
                            {total_used,131},
                            {sockets_limit,92068},
                            {sockets_used,129}]},
        {processes,[{limit,1048576},{used,1332}]},
        {run_queue,0},
        {uptime,496081}]
        ...done.

    pid 一行显示当前节点 rabbitmq-server 进程的 pid 为 12641；
    running_applications 一行显示运行的子程序有 rabbit、mnesia 等；
    memory 一行显示 rabbitmq 各组件所使用内存的情况；
    alarms 显示节点状态警告，如果有警告则节点状态出现问题；
    listeners 显示监听的端口；
    vm_memory_limit 为 rabbitmq 配置的可用内存上限，高于该上限时报警；
    disk_free_limit 为 rabbitmq 配置的剩余磁盘下限，低于该下限时报警；
    file_descriptors 为 rabbitmq 可用文件描述符上限，以及当前使用的文件描述符数目；
    processes 为 rabbitmq 可用进程数目上限，以及当前已使用进程数；
    uptime 为运行时间。

# 查看集群状态

        # rabbitmqctl cluster_status

        Cluster status of node 'rabbit@rabbit' ...
        [{nodes,[{disc,['rabbit@rabbit1','rabbit@rabbit2',
                        'rabbit@rabbit3','rabbit@rabbit4',
                        'rabbit@rabbit']}]},
        {running_nodes,['rabbit@rabbit1','rabbit3@rabbit2',
                        'rabbit@rabbit3','rabbit1@rabbit4',
                        'rabbit@rabbit']},
        {cluster_name,<<"rabbit@rabbit">>},
        {partitions,[]}]
        ...done.

    返回结果中，nodes 一行是本集群所有的节点名称，running_nodes 则是运行状态的节点名称。如果某一节点故障，则缺席这两个列表。抛开所有节点不谈，只要 partitions 不为空，则集群状态已出现故障。

# 查看用户

查看虚拟主机列表：

    # rabbitmqctl list_vhosts

EayunStack 中 rabbitmq 只有一个默认虚拟主机,名称为"/"

查看用户列表：

    # rabbitmqctl list_users

EayunStack 中 rabbitmq 只有一个用户帐户，名称为 nova，其密码为部署过程中生成的随机密码。在/etc/rabbitmq/rabbitmq.config 中可以看到默认用户名和密码，示例：

    {default_user,        <<"nova">>},
    {default_pass,        <<"hL6rF31w">>},

# 查看策略

EayunStack 中的 rabbitmq 只定义了一个策略，即所有的队列在所有节点上做镜像。查看策略：

    # rabbitmqctl list_policies
    Listing policies ...
    /       ha-all  all     .       {"ha-mode":"all","ha-sync-mode":"automatic"}    0
    ...done.

# 报告集群各方面的状态信息

该命令可以生成集群状态的完整报告：

    # rabbitmqctl report

可以将该报告输出至文件中，提供研发人员查看：

    # rabbitmqctl report > rabbitmq_report_$HOSTNAME.txt

# 启动停止 rabbit 程序

通常情况下启停 rabbitmq 程序请使用 Pacemaker 集群管理工具，以下方法仅限于 debug 时使用。以下命令是独立于 Pacemaker 管理程序来启动/停止 rabbitmq。通过这种方式的时候，Pacemaker 管理程序将察觉不到 rabbitmq 状态的变化。

> ## 警告
> 以下命令会导致 rabbitmq 服务中断。

停止：

    # rabbitmqctl stop_app

启动：

    # rabbitmqctl start_app
