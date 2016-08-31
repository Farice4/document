# 监控服务用于实时监控 EayunStack 平台状态

> ##### 注意
>
> 实时监控服务需要 ```ssmtp``` 的支持，需要提前安装 ```ssmtp``` 包，并配置好 ```/etc/ssmtp/ssmtp.conf```文件。

为了实时监控 EayunStack 平台状态，我们基于 EayunStack Tools 添加了监控服务。

## 配置服务

修改配置文件 ```/etc/eayunstack/eayunstack-tools-monitor.conf```

> ###### 注意
>
> ```Interval=``` 用于定义监控服务对环境进行状态检查的间隔时间
>
> ```MailTo=``` 用于定义当检测结果中包含 ```WARN``` 或 ```ERROR``` 级别的消息输出时，将这些消息自动发送到指定的邮箱。可以指定多个邮箱，使用 ```,``` 分隔。

```
(fuel)# vim /etc/eayunstack/eayunstack-tools-monitor.conf
# run check cmd interval (default:300)
Interval=300
# auto send alert mail to this mail list, split by ',' (default:None)
#MailTo=
```

## 启动服务

```
(fuel)# systemctl start eayunstack-tools-monitor
```

> ##### 注意
>
> 当服启动成功后，配置文件中 MailTo 指定的邮箱中可以收到一封通知邮件，依此可以确认监控服务已经正常启动，并且可以正常发送报警邮件。
>
> 服务启动失败或启动成功但是邮箱中未收到通知邮件，可以查看 ```/var/log/eayunstack-tools-monitor.log``` 确认具体原因。

## 查看服务状态

```
(fuel)# systemctl status eayunstack-tools-monitor
```

## 设置服务在系统启动时自动启动

```
(fuel)# systemctl enable eayunstack-tools-monitor
```
