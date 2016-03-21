## 删除对象
### 描述
管理员直接删除存在的对象

### 请求信息:
#### 请求格式
```
DELETE /admin/bucket?object&format=json&object=foo.txt&bucket=lilei HTTP/1.1
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
| **bucket** | 描述: 桶的名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: lucy | 是 |
| **object** | 描述: 对象的名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: foo.txt | 是 |

#### 请求消息元素
该请求中无请求消息元素

### 响应信息:
#### 响应格式
```
HTTP/1.1 StatusCode
Date: Date
Server: Server
Connection: close
Transfer-Encoding: chunked
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
DELETE /admin/bucket?object&format=json&object=foo.txt&bucket=lilei HTTP/1.1
Host: obs.eayun.com
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Wed, 25 Nov 2015 04:15:53 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:6HODFqO+ZzVedm6pQNWL0Ks3yM0=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Wed, 25 Nov 2015 04:15:53 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph3; path=/
Cache-control: private
```
