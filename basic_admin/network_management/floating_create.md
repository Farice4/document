# 创建floating ip

### 通过Web horizon创建Floating IP

* 登录Web horizon点击项目----compute-----访问&安全-----浮动IP----分配IP给项目

![Floating_Create1](../Picture/floatingip_create1.jpg)

* IP地址创建完成后，在浮东IP栏显示新创建IP地址

![Floating_Create1](../Picture/floatingip_create2.jpg)

### 通过命令创建floating ip

* 创建floating ip，执行如下命令

> ```nova floating-ip-create <floating-ip-pool>```

### 示例

* 查看floating-ip-pool

```
# nova floating-ip-pool-list
+------------------+
| name             |
+------------------+
| external_network |
| EayunStackTest   |
| EayunStack_ext   |
| net04_ext        |
| out              |
+------------------+

```

* 创建floating ip地址

```
# nova floating-ip-create EayunStackTest
+----------+-----------+----------+----------------+
| Ip       | Server Id | Fixed Ip | Pool           |
+----------+-----------+----------+----------------+
| 9.9.9.58 | -         | -        | EayunStackTest |
+----------+-----------+----------+----------------+

```
* 查看创建结果

```
# nova floating-ip-list
+------------+-----------+-----------+----------------+
| Ip         | Server Id | Fixed Ip  | Pool           |
+------------+-----------+-----------+----------------+
| 9.9.9.57   | -         | 5.5.5.7   | EayunStackTest |
| 25.0.0.197 | -         | 7.7.7.100 | net04_ext      |
| 9.9.9.51   | -         | -         | EayunStackTest |
| 25.0.0.219 | -         | 7.7.7.106 | net04_ext      |
| 9.9.9.54   | -         | -         | EayunStackTest |
| 25.0.0.220 | -         | -         | net04_ext      |
| 9.9.9.56   | -         | 5.5.5.6   | EayunStackTest |
| 25.0.0.226 | -         | 4.4.4.101 | net04_ext      |
| 9.9.9.58   | -         | -         | EayunStackTest |
+------------+-----------+-----------+----------------+
```
