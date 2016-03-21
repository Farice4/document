## 删除密钥对
### 描述
删除用户的 key 对

### 请求信息:
#### 请求格式
```
DELETE /admin/user?key&format=json HTTP/1.1
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
| **access-key** | 描述: 访问 key <br>类型: 字符串 <br>默认值: 随机不重复 <br>示例: ABCD0EF12GHIJ2K34LMN | 是 |
| **uid** | 描述: 用户的 ID <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 否 |
| **subuser** | 描述: swift 的用户ID <br>类型: 字符串 <br>默认值:  <br>示例: TODO | 是 |
| **key-type** | 描述: 创建的 key 类型 <br>类型: 字符串 <br>默认值: s3 <br>示例: s3 | 否 |

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
DELETE /admin/user?format=json&access-key=N0RCLE9CEQ0DR02AZYDH&key HTTP/1.1
Host: obs.eayun.com:9090
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Mon, 23 Nov 2015 07:41:59 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:4Au/Ow/ovGKuHRDQc8CfVxtf4DA=
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
