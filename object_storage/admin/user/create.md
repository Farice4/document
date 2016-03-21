## 创建用户
### 描述
创建一个新的用户, 默认情况下会创建一个 AK(Access Key ID) 和 SK(Secret Access Key) 密钥对并返回. 如果只是指定了access-key 或者 secret-key其中的一个, 那么另外一个key会自动生成.
> ## 警告
> 如果修改了指定的 access-key 存在于系统中, 那么会修改拥有 access-key 的这个用户.

### 请求信息:
#### 请求格式
```
PUT /admin/user?format=json&uid=UserID HTTP/1.1
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
| **uid** | 描述: 被创建用户的 ID <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 是 |
| **display-name** | 描述: 被创建的显示名字 <br>类型: 字符串 <br>默认值: 空字符串 <br>示例: test user | 是 |
| **exclusive** | 描述: 创建 UID 唯一的用户 <br>类型: Boolean <br>默认值: False <br>示例: True | 是 |
| **email** | 描述: 被创建用户的邮箱  <br>类型: 字符串 <br>默认值: 空字符串 <br>示例: test_user@eayun.com  | 否 |
| **key-type** | 描述: 创建的 key 类型 <br>类型: 字符串 <br>默认值: s3 <br>示例: s3 | 否 |
| **access-key** | 描述: 访问 key <br>类型: 字符串 <br>默认值: 随机不重复 <br>示例: ABCD0EF12GHIJ2K34LMN | 否 |
| **secret-key** | 描述: 密钥 key  <br>类型: 字符串 <br>默认值: 随机不重复 <br>示例: 0AbCDEFg1h2i34JklM5nop6QrSTUV+WxyzaBC7D8 | 否 |
| **user-caps** | 描述: 用户读写等权限 <br>类型: 字符串 <br>默认值: 空 <br>示例: usage=read, write; users=read | 否 |
| **generate-key** | 描述: 创建一对空的key加到用户中 <br>类型: Boolean <br>默认值:  <br>示例: True | 否 |
| **max-buckets** | 描述: 用户能创建的最大bucket数目 <br>类型: Integer <br>默认值:1000 <br>示例: 500 | 否 |
| **suspended** | 描述: 暂停用户 <br>类型: Boolean <br>默认值: False <br>示例: True | 否 |

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

{"user_id":UserID,"display_name":DisplayName,"email":Email,"suspended":0,"max_buckets":number,"subusers":[],"keys":[keys],"swift_keys":[],"caps":[]}
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **user_id** | 新创建用户的 ID<br>类型:字符串 |
| **display_name** | 新创建用户的显示名字<br>类型:字符串 |
| **suspended** | 新创建用户是否被暂停服务<br>类型:Boolean |
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
PUT /admin/user?format=json&uid=lucy&display-name=lucy HTTP/1.1
Host: obs.eayun.com
Content-Length: 0
Accept: */*
User-Agent: python-requests/1.1.0 CPython/2.7.5 Linux/3.10.0-123.20.1.el7.x86_64
date: Mon, 16 Nov 2015 10:08:23 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:DkbQbDxRRbAyxzBCDjxjgZR8qkY=
```
#### 响应实例
```
HTTP/1.1 200
Date: Mon, 16 Nov 2015 10:09:15 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Cache-control: private

{"user_id":"lucy","display_name":"lucy","email":"","suspended":0,"max_buckets":1000,"subusers":[],"keys":[{"user":"lucy","access_key":"F5KKUSWWP63ASA2S6HI7","secret_key":"PHQ1RMM+s066tJi1E61Y4ddxIPutrC1\/i0+2pyIb"}],"swift_keys":[],"caps":[]}
```
