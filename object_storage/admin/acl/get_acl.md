## 获取桶和对象的权限
### 描述
获取桶和对象的权限.

> ###### 注意
> 目前 admin 的 API 和管理工具只提供了权限的查看, 没有设置权限的功能, 因为从系统的角度来看, 权限的设置是有改对象的拥有这来设置的, 所以设置权限的功能请参考用户手册和开发者文档的相关章节

### 请求信息:
#### 请求格式
```
GET /admin/bucket?format=json&policy&object=ObjectName&bucket=BucketName HTTP/1.1
Host: obs.eayun.com
date: Date
Authorization: auth
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
// TODO: 格式 FIX, 并且 XML 转义了 (:
> ###### 注意
> 目前该 API 只支持 XML 的格式

| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **bucket** | 描述: bucket名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: lucy| 是 |
| **object** | 描述: object 的名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-19 16:00:00| 否 |

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

[PolicyInfo]
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **ID** | 用户的ID，即UID<br>类型: 字符串|
| **DisplayName** | 用户名称<br>类型:字符串|
| **Permission** | 该用户对于该对象设置的权限<br>类型:字符串|
| **AccessControlList** | 访问控制列表标签<br>类型:容器|
| **Grant** | 用户机器权限的标签<br>类型:容器|
| **Grantee** | 用户信息标签<br>类型:容器|

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/bucket?format=xml&policy&bucket=m8x HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.6.0 CPython/2.7.5 Linux/3.10.0-123.el7.x86_64
Connection: keep-alive
date: Tue, 08 Dec 2015 08:52:48 GMT
Authorization: AWS 9UG6JJ8JE4H0JEDHYNWF:Q4qwz2YS2GMp9j9VaHL7FGNMoXM=
```

#### 响应实例
```
HTTP/1.1 200 OK
Date: Tue, 08 Dec 2015 08:52:48 GMT
Server: Apache/2.4.6 (CentOS)
x-amz-request-id: tx00000000000000000005c-0056669a60-568fc-a
Keep-Alive: timeout=5, max=100
Connection: Keep-Alive
Transfer-Encoding: chunked
Content-Type: application/xml

<?xml version="1.0"?>
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>lucy</ID>
    <DisplayName>lucy</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>lucy</ID>
        <DisplayName>lucy</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```
