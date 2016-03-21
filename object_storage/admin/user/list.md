## 列出所有用户
### 描述
列出所有存在 EayunOBS 中的用户
### 请求信息:
#### 请求格式
```
GET /admin/metadata/user?format=json HTTP/1.1
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

["lucy","hangmeimei","admin"]
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
该消息直接返回一个包行所有用户 ID 的数组

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/metadata/user?format=json HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Connection: keep-alive
Content-Length: 0
Accept: */*
User-Agent: python-requests/1.1.0 CPython/2.7.5 Linux/3.10.0-123.20.1.el7.x86_64
date: Mon, 16 Nov 2015 10:08:23 GMT
Authorization: 8XHJDGE7265UBJP43DIT:JSbxBaMk9MzujlPTQoSrKh3qo/I=
```
#### 响应实例
```
HTTP/1.1 200
Date: Thu, 19 Nov 2015 02:25:23 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Cache-control: private

["lucy","hangmeimei","admin"]
```
