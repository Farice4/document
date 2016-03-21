## 删除使用量信息
### 描述
删除相应的使用量信息

### 请求信息:
#### 请求格式
```
DELETE /admin/usage?format=json HTTP/1.1
Host: obs.eayun.com
date: Date
Authorization: auth
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **format** | 描述: 服务器返回内容的格式 <br>类型: 字符串<br>默认值: json<br>示例: json| 否 |
| **uid** | 描述: 用户 ID, 如果没有指定用户 ID, 删除所有用户的使用量 <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 否 |
| **start** | 描述: 查询使用量的开始日期和时间 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-10 16:00:00| 否 |
| **end** | 描述: 查询使用量的结束日期和时间 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-19 16:00:00| 否 |
| **remove-all** | 描述: 是否删除所有所有用户的信息<br>类型: Boolean <br>默认值: True <br>示例: False| 否 |

> ## 警告
> 如果同时传递了 **uid** 和 **remove-all** 参数, 则 **uid** 参数不生效, 会删除所有的信息

#### 请求消息元素
该请求中无请求消息元素

### 响应信息:
#### 响应格式
```
HTTP/1.1 StatusCode
Date: Date
Server: Server
Connection: close
Content-Type: Type
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
本消息无相应消息元素

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
DELETE /admin/usage?format=json&start=2015-11-17&end=2015-11-27 HTTP/1.1
Host: obs.eayun.com:9090
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Mon, 23 Nov 2015 03:30:14 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:g6COV4FJDPaK1Y3n9fsW9/5n6JM=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Mon, 23 Nov 2015 03:30:14 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private
```
