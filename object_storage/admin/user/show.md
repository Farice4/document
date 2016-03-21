## 获取用户信息
### 描述
获取用户详细信息

### 请求信息:
#### 请求格式
```
GET /admin/user?format=json HTTP/1.1
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
| **uid** | 描述: 用户 ID <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |

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

{"user_id":UserID,"display_name":DisplayName,"email":Email,"suspended":0,"max_buckets":number,"subusers":[],"keys":[keys],"swift_keys":[],"caps":[]}
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **user_id** | 用户的 ID<br>类型:字符串 |
| **display_name** | 用户的显示名字<br>类型:字符串 |
| **suspended** | 用户是否被暂停服务<br>类型:Boolean |
| **max_buckets** | 用户能创建的最大bucket数目<br>类型:字符串 |
| **subusers** | 关联到这个数组的子用户<br>类型:数组 |
| **keys** | 关联到该用户的s3 key对<br>类型:数组 |
| **swift_keys** | 关联到该用户的 swift key<br>类型:数组 |
| **caps** | 用户具有的系统权限<br>类型:数组 |

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/user?format=json&uid=lucy HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Thu, 19 Nov 2015 03:35:11 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:wy/vF+KSaZsdqT3v/BXrlCsFJD4=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Thu, 19 Nov 2015 03:35:11 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph2; path=/
Cache-control: private

{"user_id":"lucy","display_name":"lucy","email":"","suspended":0,"max_buckets":1000,"subusers":[],"keys":[{"user":"lucy","access_key":"EPL57OCTJSZB0E7EKDD8","secret_key":"O8h7XGVLHqg\/zAYoWHtmb+sodRhJwcFsWkv6I5c0"},{"user":"lucy","access_key":"F5KKUSWWP63ASA2S6HI7","secret_key":"PHQ1RMM+s066tJi1E61Y4ddxIPutrC1\/i0+2pyIb"}],"swift_keys":[],"caps":[]}
```
