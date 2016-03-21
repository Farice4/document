## 获取配额
### 描述
获取配额信息, 包括两种配额信息, 用户配额和bucket配额, 在 API 中使用 quota-type 和 quota-scope 参数类区分

### 请求信息:
#### 请求格式
```
GET /admin/user?quota&uid=<uid>&quota-type=user
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
| **uid** | 描述: 用户 ID<br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |
| **quota-type** | 描述: 配额类型, 只能为 user或者bucket之一 <br>类型: 字符串 <br>默认值: 无 <br>示例:user | 是 |

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

[response-data]
```

> ###### 注意
> 为了方便显示, 返回的 json 字符串做了排版

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **enabled** | 是否启用配额 <br>类型: Boolean|
| **bucket** | bucket名字 <br>类型: 字符串|
| **max_objects** | 允许使用的最大的对象数量, 如果为负, 则不限制 <br>类型: 整形|
| **max_size_kb** | 允许使用的最大的对象字节数, 如果为负, 则不限制 <br>类型: 整形|
| **quota-scope** | 配额类型, 只能为 user或者bucket之一 <br>类型: 字符串|

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/user?format=json&uid=testobs&quota&quota-type=bucket HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Tue, 15 Dec 2015 07:15:33 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:eK12t3VMoxR4/UFelpLoOWEvcTs=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Tue, 15 Dec 2015 07:15:33 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private

{"enabled":true,"max_size_kb":1000,"max_objects":5}
```
