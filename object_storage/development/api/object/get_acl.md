## 获取对象ACL

### 描述

在开启多版本的情况下，默认获取的是最新版本对象的ACL。

### 请求信息:

#### 请求格式

```
GET /BucketName/ObjectName?acl HTTP/1.1
Host: obs.eayun.com
Date: date
Authorization: auth
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **acl**  |  指明是获取对象的ACL  |   是   |
|  **versionId**  |  指定获取对象的版本号；<br>如versionId=1  |   否   |
    
#### 请求消息元素

该请求中无请求消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
Content-Length: length
Content-Type: type

<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Owner>
<ID>id</ID>
<DisplayName>name</DisplayName>
</Owner>
<AccessControlList>
<Grant>
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
<ID>id</ID>
<DisplayName>name</DisplayName>
</Grantee>
<Permission>permission</Permission>
</Grant>
</AccessControlList>
</AccessControlPolicy>
```

#### 响应消息头

| 消息头名字 | 描述 |
|  :---  |  :---  |
|  **x-amz-version-id**  |  对象的版本号 |

#### 响应消息元素

| 元素名字 | 描述 |
|  :---  |  :---  |
|  **ID**  |  用户的ID，即UID  |
|  **DisplayName**  |  用户名称  |
|  **Permission**  |  该用户对于该对象设置的权限  |
|  **AccessControlList**  |  访问控制列表标签  |
|  **Grant**  |  用户机器权限的标签  |
|  **Grantee**  |  用户信息标签  |

#### 错误响应消息

请参考错误响应描述

### 示例:

获取对象/kmnt/outkey的acl。

#### 请求实例

```
GET /outkey?acl HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:hUTBv4n6n36SFnADkcRqSZbXAOo=
Date: Thu, 19 Nov 2015 06:33:25 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 200 
Date: Thu, 19 Nov 2015 06:33:44 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph3; path=/
Cache-control: private

<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Owner>
<ID>testuser</ID>
<DisplayName>testuser</DisplayName>
</Owner>
<AccessControlList>
<Grant>
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"xsi:type="CanonicalUser">
<ID>testuser</ID>
<DisplayName>testuser</DisplayName>
</Grantee>
<Permission>FULL_CONTROL</Permission>
</Grant>
</AccessControlList>
</AccessControlPolicy>
```