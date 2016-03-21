## 创建密钥对
### 描述
给用户添加一个新的 key 对, 如果指定了 access-key 或者 secret-key 中的一个, 另外的key则会自动生成.
> ## 警告
> 如果修改了指定的 access-key 存在于系统中, 那么会修改拥有 access-key 的这个用户.

### 请求信息:
#### 请求格式
```
PUT /admin/user?key&format=json&uid=UserID HTTP/1.1
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
| **uid** | 描述: 用户的 ID <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |
| **subuser** | 描述: swift 的用户ID <br>类型: 字符串 <br>默认值:  <br>示例: TODO | 是 |
| **key-type** | 描述: 创建的 key 类型 <br>类型: 字符串 <br>默认值: s3 <br>示例: s3 | 否 |
| **access-key** | 描述: 访问 key <br>类型: 字符串 <br>默认值: 随机不重复 <br>示例: ABCD0EF12GHIJ2K34LMN | 否 |
| **secret-key** | 描述: 密钥 key  <br>类型: 字符串 <br>默认值: 随机不重复 <br>示例: 0AbCDEFg1h2i34JklM5nop6QrSTUV+WxyzaBC7D8 | 否 |
| **generate-key** | 描述: 创建一对空的key加到用户中 <br>类型: Boolean <br>默认值: True <br>示例: True | 否 |

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

[{"user":UserID,"access_key":Access_Key,"secret_key":Secret_Key"},{"user":UserID,"access_key":Another_Access_Key,"secret_key":Another_Secret_Key"}]
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **user** | 用户ID<br>类型:字符串 |
| **access-key** | 访问 key<br>类型:字符串 |
| **secret-key** | 密钥 key<br>类型:字符串 |

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
PUT /admin/user?key&format=json&uid=lucy HTTP/1.1
Host: obs.eayun.com:9090
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Mon, 23 Nov 2015 03:59:39 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:qzBA6EErihQb1ZroYXKtdUNamEU=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Mon, 23 Nov 2015 03:59:39 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph2; path=/
Cache-control: private

[{"user":"lucy","access_key":"7WESQW3X1VHYXR5SC8XW","secret_key":"Uu79puVb7xdjuVy62YMvRSzjKB1m3\/\/bXGy0\/Mz\/"},{"user":"lucy","access_key":"ILI6079F5LDC4EBVJ772","secret_key":"nyLypAucBW2lCamVHsMTyZylzL56bCL3oDPWM9sf"}]
```
