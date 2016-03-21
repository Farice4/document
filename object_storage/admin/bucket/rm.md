## 删除桶
### 描述
删除存在的桶, 如果桶中存在对象且请求中没有带 **purge-objects** 参数或者该参数为 False, 则删除失败, 系统返回 **BucketNotEmpty** 错误

### 请求信息:
#### 请求格式
```
DELETE /admin/bucket?format=json&bucket=BucketID HTTP/1.1
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
| **bucket** | 描述: bucket 的名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: lucy | 是 |
| **purge-objects** | 描述: 是否强制删除此 bucket 下的对象 <br>类型: Boolean <br>默认值: False <br>示例:True | 否 |

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
DELETE /admin/bucket?format=json&purge-objects=True&bucket=lucy2 HTTP/1.1
Host: obs.eayun.com
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Wed, 25 Nov 2015 03:23:47 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:7imf9Lsq8TOmQMm04bQw9ysqCZo=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Mon, 23 Nov 2015 07:41:59 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph2; path=/
Cache-control: private
```
