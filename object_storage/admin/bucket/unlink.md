## 删除桶到用户的链接
### 描述
将一个桶链接到其他用户, 此操作将该桶从原来桶的拥有者中删除.

//TODO link, unlink 需要再看代码搞清意图
### 请求信息:
#### 请求格式
```
POST /admin/bucket?format=json&bucket=BucketName&uid=UserID HTTP/1.1
Host: obs.eayun.com
date: Date
Authorization: auth
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
// TODO: stats 参数好像有 bug, 指定 false 也返回所有
| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **format** | 描述: 服务器返回内容的格式 <br>类型: 字符串<br>默认值: json<br>示例: json| 否 |
| **bucket** | 描述: bucket名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: lucy| 是 |
| **uid** | 描述: 用户 ID<br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |

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
POST /admin/bucket?format=json&bucket=m8x&uid=lucy HTTP/1.1
Host: obs.eayun.com
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Wed, 25 Nov 2015 03:52:24 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:lmTWl09U6T/S7TDx+ETL9fGwLgI=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Wed, 25 Nov 2015 03:52:24 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph2; path=/
```
