### 获取桶的访问控制列表
#### 描述
获取桶的访问控制列表，要求发起请求的用户是桶的所有者或者针对该桶具有 `READ_ACP` 权限。

#### 请求信息
```
GET /{bucket}?acl HTTP/1.1
Host: obs.eayun.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

#### 请求消息头
##### 公共请求消息头
请参考[公共消息头](../header.md)

#### 请求消息参数
该请求无请求消息参数

#### 请求消息元素
该请求无请求消息元素

#### 响应消息
##### 响应格式
```
HTTP/1.1 {StatusCode}
Date: {Date}
Server: {Server}
Content-Type: application/xml

<?xml version="1.0" ?><AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Owner><ID>{BucketOwnerID}</ID><DisplayName>{BucketOwnerDisplayName}</DisplayName></Owner><AccessControlList><Grant><Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser"><ID>{GranteeID}</ID><DisplayName>{GranteeDisplayName}</DisplayName></Grantee><Permission>{Permission}</Permission></Grant>...</AccessControlList></AccessControlPolicy>
```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
| 元素名称 | 描述 |
| :-- | :-- |
| AccessControlList | ACL 配置的 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy` |
| AccessControlPolicy | 响应消息最上层的的 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：无 |
| DisplayName | 桶所有者的显示名称。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy.Owner` |
| Grant | 响应消息元素 `Grantee` 和 `Permission` 的上层 XML 容器。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy.AccessControlList` |
| Grantee | 对应一个被赋予权限的用户，包含两个次一层的元素 `DisplayName` 和 `ID` （要赋予权限的用户的 `DisplayName` 和 `ID` ）。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy.AccessControlList.Grant` |
| ID | 桶所有者的 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy.Owner` |
| Owner | 桶所有者相关信息的 XML 容器。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy` |
| Permission | `Grantee` 指定用户对当前桶的所拥有的权限。 <br /> 类型：字符串 <br /> 有效值：`FULL_CONTROL` ，`WRITE` ，`WRITE_ACP` ，`READ` 以及 `READ_ACP` <br /> 上层数据结构：`AccessControlPolicy.AccessControlList.Grant` |

##### 错误响应消息
请参考[错误响应描述](../error.md)

#### 示例
##### 请求示例
```
GET /new-bucket?acl HTTP/1.1
Host: ceph
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Fri, 27 Nov 2015 07:54:52 GMT
Authorization: AWS 9FSTXQR54VKR7VEM7GAJ:kyZQ7ihshvLKwTc4IAXCTYmBRdE=
```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Cache-control: private
Date: Fri, 27 Nov 2015 07:54:48 GMT
Content-Type: application/xml

<?xml version="1.0" ?>
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Owner>
        <ID>zc-test-1</ID>
        <DisplayName>Testing User for ZhaoChao</DisplayName>
    </Owner>
    <AccessControlList>
        <Grant>
            <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
                <ID>zc-test-1</ID>
                <DisplayName>Testing User for ZhaoChao</DisplayName>
            </Grantee>
            <Permission>FULL_CONTROL</Permission>
        </Grant>
    </AccessControlList>
</AccessControlPolicy>
```
