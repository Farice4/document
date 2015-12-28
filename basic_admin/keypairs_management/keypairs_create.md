# 创建新密钥

### 通过Web horizon创建密钥

* 登录Web horizon登录，点击访问&安全----密钥对----创建密钥对

![KeyPairs_Create](../Picture/keypairs_create1.jpg)

* 输入密钥对名称----点击创建密钥对，密钥对创建成功

![KeyPairs_Create](../Picture/keypairs_create2.jpg)

* 创建的密钥对，第一次可以下载保存下来，用于后面进行ssh登录时使用。在创建新的云主机时可以选择访问&安全----值对----选择创建的密钥对，注入到云主机中

![KeyPairs_Create](../Picture/keypairs_create3.jpg)


### 通过命令创建密钥对

* 创建密钥对，执行如下命令

> ```nova keypair-add [--pub-key <pub-key>] <name>```

* 创建密钥时，可以将公钥存储到文件

> ```nova keypair-add [--pub-key <pub-key>] <name> > Test.pem```

### 示例

* 直接创建公钥将会打印到屏幕

```
# nova keypair-add Test3
-----BEGIN RSA PRIVATE KEY-----
MIIEowIBAAKCAQEA5KdKcP8FdO+s56G9+Nb1+BrWXSZBGJu6eAdydK/da54VO6PL
LWGjHgk5jUS7p2wgqxdWyKrE40w67O9TTe/Dug+DbyJH3XoCeBhOSKuMo8FxwlTQ
UewIzsxKKPWNaEYBWe0YYkgz2YoOwLSWDZUiyrdY5FqO/nvg3SN5MCkMR5RybYtO
rAdQRh9azZvvqBEWiggcvuDDUdFPj7LTK/PDZXj0yy83CkDccdKgrYTg5hoglrh/
N6GCekU6YhRjR9OjeXeKy+8sPb70ZQlMtCvTJchelmdNP4hd6gNZOeuYfmrvCSmI
pB9i0BQmYRmrF6btr2SNfvXayvhymE9a3F8ulQIDAQABAoIBABGHQY2n/yMXvImF
PKr2z6RVall5tUPNrF3HNF79nFvDop0P0WOJOJf03RSXUEtlXpu/DkgTz0Qdc+b1
8czntHlu9wRt/FReJhJOyrGHdSCYk7/yAIZHMeWog4bgTBbSRwg9xMDECDWE/9yF
+3rPE/u2Unou9wdLN9G8Bm5Kt0ZFVd+NZ0wBw+6sH2cTzjz0uz3pf9vIfyXFh7RA
oA3QR2B4uUMXWdC3D4TQppZoIkgx6l/UmwU+oZXevrgmy0/0G7e2YpMRSbIavufS
WTGmYe3B5uka9CwhYlypjVItFkPZeU275tOw/07qoOkTqC8Gv5VUzVDhWC2yuF/j
vOLYMAECgYEA/HOYe8pG3dIB+LCxlMmgX124hcYVtmYAjyHRiGpsukImYohQRHLo
imZrdQbCXq0IcVVdisjLsbGPtKJTP4/53G7Vb09psfy/LdlYkTLq2T5/YYwb9Tef
JSuMl0utH+Sg4kaJZiuAP/qNggA+gVSt06tnFHoLggZCp2UaUcpYaWECgYEA594P
0ze12fkA8kFwmzzcux1k5pnLTwwwRNKNGRpgc+b8eGKixiebKX2I1N8UaO6qAyQO
lIrg4az49ReGQyumopEMYr/+Tg45nAUaqv7UXFeZdsW3GbVIr+AZTmCrShTpLQEj
KdRZrxFZz2ZUk5y1gPED3DHTDqBIPGRSjM+6zbUCgYAjzAxqOUOVPqsy5XM44uJP
w41fwrffdlVL9oKsslKakri4rhWbryOURHOmU4AKRsT0tSMfg9uGZeQ3u/DMCZFn
lXvEXeFh8LH3QWOSq0Kr7XXIkkKAohN6BNPZvExfQl8h1tiy2SZFbDTWo0w5sJU5
d4uVVrz9AgA/llGGGYgx4QKBgQCQVpcpWCvXRf/FzuDnckrCHHk8aA685YyCu0Nk
lhvm6qrN0jEcA3m+qUvtn/UXoUKqc96zr4RwV/oesIc+90RjPMImEvie5JIOQsso
GTxfQ5Tf8BKcaVKXUXzPYrQeiMfi4StflbZCmh17U2tg/JOscdyCTW3iU/rDxXvx
9jqGZQKBgH/340karqvETh7BQNUg220uNV/3cUylF4AmT1MBUL1MfJMo7XSATBuh
fro65HCHgZR8NW3R3rSYdbEA/hC7e6Gy8c44MP9xVH1btvklZnAeVeAlnNzgR/EJ
LOD/1GOdViu6INSWbknaYl0YUjkGVNeTT6RXSJkAMv2okBv98QZt
-----END RSA PRIVATE KEY-----

```

* 创建密钥，将公钥保存到文件

```
# nova keypair-add Test4 > test4.pem
# nova keypair-list
+------------+-------------------------------------------------+
| Name       | Fingerprint                                     |
+------------+-------------------------------------------------+
| Eayunstack | 54:28:69:49:e1:98:53:a8:5c:0d:48:61:af:bb:aa:42 |
| System     | ba:db:bc:12:a9:62:06:05:6c:00:89:02:e3:f6:aa:7d |
| Test1      | b8:ce:5a:30:f6:fd:99:64:f4:67:e9:e8:9e:c7:ee:d8 |
| Test2      | c8:66:c0:11:24:46:a7:0b:63:ef:1a:bf:a0:60:2d:51 |
| Test3      | 14:b6:ad:9d:ff:1c:8d:80:e8:f8:7f:13:c5:32:29:f4 |
| Test4      | 2b:ae:50:eb:c7:36:3f:2f:ce:40:40:e8:7e:04:92:b1 |
+------------+-------------------------------------------------+

```
