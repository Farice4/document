## 设置对象ACL

### 描述

在开启多版本的情况下，默认的是操作最新版本对象的ACL。

### 请求信息:

#### 请求格式

```
PUT /BucketnName/ObjectName?acl HTTP/1.1
Host: obs.eayun.com
Date: date
Authorization: auth

<AccessControlPolicy>
<Owner>
<ID>ID</ID>
<DisplayName>displayname</DisplayName>
</Owner>
<AccessControlList>
<Grant>
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">grantee</Grantee>
<Permission>permission</Permission>
</Grant>
</AccessControlList>
</AccessControlPolicy>
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **acl**  |  指明是操作对象的acl  |   是   |
|  **versionId**  |  指定获取对象的版本号；<br>如versionId=1  |   否   |
    
#### 请求消息元素

| 元素名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **ID**  |  用户的ID，即UID  |   否   |
|  **DisplayName**  |  用户名称  |   否   |
|  **Permission**  |  该用户对于该对象设置的权限  |  否  |
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
Content-Length: length
Content-Type: type
```

#### 响应消息头

| 消息头名字 | 描述 |
|  :---  |  :---  |
|  **x-amz-version-id**  |  设置后的对象的版本号 |

#### 响应消息元素

该响应消息无消息元素

#### 错误响应消息

请参考错误响应描述

### 示例:

#### 请求实例

```
PUT /outkey?acl HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:3ErHnSKvSBzqEVfuBgqhO1NdIDo=
Date: Thu, 19 Nov 2015 06:39:30 +0000
Content-Type: application/octet-stream
Content-Length: 401

<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Owner><ID>testuser</ID><DisplayName>testuser</DisplayName></Owner>
<AccessControlList>
<Grant>
<Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
<ID>testuser</ID><DisplayName>testuser</DisplayName>
</Grantee>
<Permission>READ</Permission>
</Grant>
</AccessControlList>
</AccessControlPolicy>
```

#### 响应实例

```
HTTP/1.1 200 
Date: Thu, 19 Nov 2015 06:39:30 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private
```