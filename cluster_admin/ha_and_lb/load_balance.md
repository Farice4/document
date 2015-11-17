# HAProxy
HAProxy 是 EayunStack 平台中实现负载均衡的服务程序。

配置文件为： `/etc/haproxy/haproxy.cfg` ，主要内容包括：

    global
      daemon
      group  haproxy
      log  /dev/log local0
      maxconn  16000
      pidfile  /var/run/haproxy.pid
      stats  socket /var/lib/haproxy/stats
      tune.bufsize  32768
      tune.maxrewrite  1024
      user  haproxy

    defaults
      log  global
      maxconn  8000
      mode  http
      option  splice-auto
      retries  3
      stats  enable
      timeout  http-request 10s
      timeout  queue 1m
      timeout  connect 10s
      timeout  client 1m
      timeout  server 1m
      timeout  check 10s

    listen Stats *:10000
      mode http
      stats enable
      stats uri /
      stats refresh 5s
      stats show-node
      stats show-legends
      stats hide-version

    include conf.d/*.cfg

EayunStack 平台中各服务组件的具体配置在 `/etc/haproxy/conf.d` 目录下。

# <a name="eayunstack_lb_services_list" style="text-decoration: none; color: inherit;" />EayunStack 负载均衡集群中的服务
EayunStack 负载均衡集群中包含的服务有：

(说明：下表中的 `vip__public` 和 `vip__management` 对应于 EayunStack 高可用集群中的两个同名资源所管理的 IP 地址，这两个 IP 地址也分别对应 OpenStack 平台的 Public 和 Management 网络所使用的 IP 地址。）

| 服务 | 监听地址 | 说明 |
|------|----------|------|
| horizon | vip\_\_public:80 | |
| keystone-1 | vip\_\_public:5000<br />vip\_\_management:5000 | |
| keystone-2 | vip\_\_public:35357<br />vip\_\_management:35357 | |
| nova-api-1 | vip\_\_public:8773<br />vip\_\_management:8773 | |
| nova-api-2 | vip\_\_public:8774<br />vip\_\_management:8774 | |
| nova-metadata-api | vip\_\_management:8775 | |
| cinder-api | vip\_\_public:8776<br />vip\_\_management:8776 | |
| glance-api | vip\_\_public:9292<br />vip\_\_management:9292 | |
| neutron | vip\_\_public:9696<br />vip\_\_management:9696 | |
| glance-registry | vip\_\_management:9191 | |
| rabbitmq | vip\_\_management:5672 | 实际未使用 |
| mysqld | vip\_\_management:3306 | |
| ceilometer | vip\_\_public:8777<br />vip\_\_management:8777 | |
| heat-api | vip\_\_public:8004<br />vip\_\_management:8004 | |
| heat-api-cfn | vip\_\_public:8003<br />vip\_\_management:8003 | |
| heat-api-cloudwatch | vip\_\_public:8000<br />vip\_\_management:8000 | |
| nova-novncproxy | vip\_public:6080 | |
