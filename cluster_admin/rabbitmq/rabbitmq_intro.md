# RabbitMQ

RabbitMQ 是 EayunStack 所采用的消息通讯服务。

# RabbitMQ 集群

标准的 EayunStack 部署包含多台控制节点组成的 RabbitMQ 集群。多个节点拥有同等地位，共同为 EayunStack 各服务提供消息通讯，任一个节点故障的情况下，其它节点可以接管故障节点的服务。

EayunStack 中 RabbitMQ 集群节点列表在 /etc/rabbitmq/rabbitmq.config 中，例如：

    [
    ...
    {rabbit, [
            ...
            {cluster_nodes, {['rabbit@node-1', 'rabbit@node-2', 'rabbit@node-3'], disc}},
            ...
    ]},
    ...
    ].

