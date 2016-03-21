## 设置配额
### 描述
设置配额信息, 包括两种配额信息, 用户配额和bucket配额, 在 API 中使用 quota-type 和 quota-scope 参数类区分

### 请求信息:
#### 请求格式
```
PUT /admin/user?quota&uid=<uid>&quota-type=user
Host: obs.eayun.com
date: Date
Authorization: auth

[request-data]
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **format** | 描述: 服务器返回内容的格式 <br>类型: 字符串<br>默认值: json<br>示例: json| 否 |
| **uid** | 描述: 用户 ID<br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |
| **quota-type** | 描述: 配额类型, 只能为 user或者bucket之一 <br>类型: 字符串 <br>默认值: 无 <br>示例:user | 是 |

#### 请求消息元素
请求的消息必须以 json 格式传递, 具体的示例见下面的示例

| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **enabled** | 描述: 是否启用配额 <br>类型: Boolean <br>默认值: 无 <br>示例:true | 否 |
| **bucket** | 描述: bucket名字 <br>类型: 字符串<br>默认值: 无<br>示例: 100| 否 |
| **max_objects** | 描述: 允许使用的最大的对象数量, 如果为负, 则不限制 <br>类型: 整形<br>默认值: 无<br>示例: 100| 否 |
| **max_size_kb** | 描述: 允许使用的最大的对象字节数, 如果为负, 则不限制 <br>类型: 整形<br>默认值: 无<br>示例: 1000| 否 |
| **quota-scope** | 描述: 配额类型, 只能为 user或者bucket之一 <br>类型: 字符串 <br>默认值: 无 <br>示例:user | 是 |

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
PUT /admin/user?format=json&uid=testobs&quota&quota-type=user HTTP/1.1
Host: obs.eayun.com
Content-Length: 105
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Tue, 15 Dec 2015 06:57:26 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:jEyb7udTyA0o+BKiVn7z1PrQpqw=

{"max_objects": 5, "bucket": "testobs1", "quota-scope": "bucket", "enabled": "true", "max_size_kb": 1000}
```
#### 响应实例
```
HTTP/1.1 200 
Date: Tue, 15 Dec 2015 06:57:26 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private

```
