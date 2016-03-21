### 设置桶的访问控制列表
#### 描述
为桶设置一项访问控制权限，要求发起请求的用户是桶的所有者或者针对该桶具有 `WRITE_ACP` 权限。

设置桶的访问控制列表有两种方法：
* 在请求消息元素中指定访问控制权限
* 在请求消息头中指定访问控制权限
> ##### 注意
> 不能同时使用上述两种方式

#### 请求消息
如果使用在请求消息元素中指定访问控制权限，则使用下面的格式。如果要在请求消息头中指定，请查看后文中请求消息头相关的说明。
```
PUT /{bucket}?acl HTTP/1.1
Host: obs.eayun.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}

<AccessControlPolicy><Owner><ID>{BucketOwnerID}</ID><DisplayName>{BucketOwnerDisplayName}</DisplayName></Owner><AccessControlList><Grant><Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser"><ID>{GranteeID}</ID><DisplayName>{GranteeDisplayName}</DisplayName></Grantee><Permission>{Permission}</Permission></Grant>...</AccessControlList></AccessControlPolicy>
```

##### 公共请求消息头
请参考[公共消息头](../header.md)

##### 额外的请求消息头：
使用请求消息头设置桶的访问控制权限也有 2 种方法：
* 使用标准访问控制策略
* 针对不同的使用者设置具体的访问控制权限

使用标准访问控制策略的方法：

| 消息头名称 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| x-amz-acl | 标准访问控制策略。 <br /> 类型：字符串 <br /> 有效值： `private` ， `public-read` ， `public-read-write` ， `authenticated-read` <br /> 默认值：`private` | 否 |

要针对不同使用者分别设置访问控制权限，需要下列以 “x-amz-grant-” 开头的消息头：

| 消息头名称 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| x-amz-grant-read | 允许指定的使用者获取桶内对象的列表。 <br /> 类型：字符串 <br /> 默认值：无 <br /> 限制条件：无 | 否 |
| x-amz-grant-write | 允许指定的使用者在桶内创建、修改、删除对象。 <br /> 类型：字符串 <br /> 默认值：无 <br /> 限制条件：无 | 否 |
| x-amz-grant-read-acp | 允许指定的使用者读取桶的访问控制配置信息。<br /> 类型：字符串 <br /> 默认值：无 <br /> 限制条件： 无 | 否 |
| x-amz-grant-write-acp | 允许指定的使用者更改桶的访问控制配置信息。 <br /> 类型：字符串 <br /> 默认值： 无 <br /> 限制条件：无 | 否 |
| x-amz-grant-full-control | 允许指定的使用者拥有上述 `READ` ， `WRITE` ， `READ_ACP` ， `WRITE_ACP` 权限。 <br /> 类型：字符串 <br /> 默认值：无 <br /> 限制条件：无 | 否 |

上面 `x-amz-grant-` 开头的消息头在使用时，设置值的格式是以 “,” 分隔的使用者列表，指定使用者时使用 `类型=值` 的格式，支持的类型包括：
* emailAddress ，指定使用者对应的邮件地址
* id ，指定使用者的 ID
* uri ，由 uri 地址定义的使用者用户组

例如，下面的请求消息头会赋予 `AuthencicatedUsers` 用户组 `WRITE` 权限，另两个分别用 `id` 和 `emailAddress` 指定的用户有 `READ` 权限：
> x-amz-grant-write: uri="http://acs.amazonaws.com/groups/global/AuthenticatedUsers"<br />
> x-amz-grant-read: id="abc", emailAddress="xyz@eayun.com"

#### 请求消息参数
该请求无请求消息参数

#### 请求消息元素
使用请求消息元素设置桶的访问控制权限时，需要使用以下元素：
> ##### 注意
> 使用请求消息元素设置桶的访问控制权限时，不能同时通过请求消息头设置访问控制权限。

| 元素名称 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| AccessControlList | 元素 `Grant` ， `Grantee` ， `Permission` 元素的 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy` | 否 |
| AccessControlPolicy | 包含所有访问控制权限设置信息的上层 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：无 | 否 |
| DisplayName | 桶所有者的显示名称。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy.Owner` | 否 |
| Grant | 元素 `Grantee` 和 `Permission` 的上层 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy.AccessControlList` | 否 |
| Grantee | 对应一个被赋予权限的用户，包含两个次一层的元素 `DisplayName` 和 `ID` （要赋予权限的用户的 `DisplayName` 和 `ID` ）。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy.AccessControlList.Grant` | 否 |
| ID | 桶所有者的 ID 或者被赋予权限的用户的 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`AccessControlPolicy.Owner` 或者 `AccessControlPolicy.AccessControlList.Grant` | 否 |
| Owner | 桶所有者相关信息的 XML 容器。 <br /> 类型：XML 容器 <br /> 上层数据结构：`AccessControlPolicy` | 否 |
| Permission | 要为 `Grantee` 中指定的用户赋予对当前桶的权限。 <br /> 类型：字符串 <br /> 有效值：`FULL_CONTROL` ，`WRITE` ，`WRITE_ACP` ，`READ` 以及 `READ_ACP` <br /> 上层数据结构：`AccessControlPolicy.AccessControlList.Grant` | 否 |

指定用户时和使用 `x-amz-grant-` 请消息头类似，也有 3 种方式：
* 使用 ID
    ```
    <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser"><ID>ID</ID><DisplayName>GranteesEmail</DisplayName></Grantee>
    ```
* 使用邮件地址
    ```
    <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="AmazonCustomerByEmail"><EmailAddress>Grantees@email.com</EmailAddress></Grantee>
    ```
* 使用预定义的用户组
    ```
    <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group"><URI>http://acs.amazonaws.com/groups/global/AuthenticatedUsers</URI></Grantee>
    ```

#### 响应消息
##### 响应格式
```
HTTP/1.1 {StatusCode}
Date: {Date}
Server: {Server}
Content-Type: application/xml


```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
该响应无响应消息元素

#### 示例
##### 请求示例
```
PUT /new-bucket?acl HTTP/1.1
Host: obs.eayun.com
Content-Length: 589
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Mon, 30 Nov 2015 06:51:55 GMT
Authorization: AWS 9FSTXQR54VKR7VEM7GAJ:W7I1t7jPFfmjfmMSgaid/K8wrrw=

<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>zc-test-1</ID>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>zc-test-1</ID>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>zc-test-2</ID>
      </Grantee>
      <Permission>READ</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Cache-control: private
Date: Mon, 30 Nov 2015 06:51:55 GMT
Content-Type: application/xml


```
