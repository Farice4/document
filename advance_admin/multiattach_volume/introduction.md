# multiattach 卷介绍

Cinder multiattach 卷可以由多个虚拟机同时挂载，适用传统应用的集群架构。

## 创建 multiattach 卷
创建 multiattach 卷只在使用 volumev2 api 的 cinderclient 代码中实现，因此需要确认 cinderclient 使用 volumev2 api ：
```
# export OS_VOLUME_API_VERSION=2
# cinder create --name <volume-name> --allow-multiattach 1
```

## 使用 multiattach 卷
multiattach 卷的使用和普通卷区别不大，有 2 种情况：
* 创建虚拟机时直接挂载使用；
* 在已有的虚拟机上附加 multiattach 卷。
