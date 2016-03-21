### 创建桶
#### 描述
创建一个新桶。创建时必须提供有效的用户 ID 及对应的 Access Key 进行身份验证。匿名用户无法创建新桶。

#### 限制条件
* 桶名称必须是全局惟一；
* 桶名称的长度在 3 ～ 255 之间（包括 3 和 255 ）；
* 桶名称的必须以数字或者字母开头；
* 桶名称只能由数字、字母、“-”、“.”组成。

创建桶时可以设置桶的访问控制权限，访问控制权限通过请求消息头进行。

创建桶可以指定桶所属的数据中心以及数据存放规则，通过请求消息元素 `CreateBucketConfiguration` 进行配置。


#### 请求信息
##### 请求格式
```
PUT /{bucket} HTTP/1.1
Host: obs.eayun.com
x-amz-acl: public-read-write

Authorization: AWS {access-key}:{hash-of-header-and-secret}

<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><LocationConstraint>BucketRegion[:BucketPlaceMentRule]</LocationConstraint></CreateBucketConfiguration>
```

#### 请求消息头
##### 公共请求消息头
请参考[公共消息头](../header.md)

##### 额外的请求消息头：
使用请求消息头设置桶的访问控制权限有 2 种方法：
* 使用标准访问控制策略
* 针对不同的使用者设置具体的访问控制权限
> ###### 注意
> 不能同时使用上述两种方式

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
| 元素名称 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| CreateBucketConfiguration | 请求消息元素的最上层 XML 容器，包含 `LocationConstraint` 元素。 <br /> 类型：XML 容器 <br /> 上层数据结构：无 | 否 |
| LocationConstraint | 指定桶所属的数据中心以及数据数储策略。<br /> 格式：数据中心[:数据存储策略]，数据存储策略可以不指定。 <br /> 上层数据结构：`CreateBucketConfiguration` | 否 |

#### 响应消息
##### 响应格式
```
HTTP/1.1 {StatusCode}
Date: {Date}
Server: {Server}
Connection: close
Content-Type: application/xml


```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
下面 2 种情况下：
1. 桶名称全局惟一；
2. 桶名称已经存在，但发起请求的用户是该桶的所有者
创建桶的操作成动，无任何响应消息元素。

##### 错误响应消息
如果桶名称已经被其它用户使用，则创建桶操作失败，错误响应如下：

| HTTP 状态码 | 描述 | 错误码 |
| :-- | :-- | :-- |
| 409 | 请求创建的桶名称已经存在，而且为其它用户所有，选择一个不同的名字重新尝试。| BucketAlreadyExists |

错误响应消息格式
```
HTTP/1.1 409
Date: {Date}
Server: {Server}
Connection: close
Content-Type: application/xml

<?xml version="1.0" encoding="UTF-8"?><Error><Code>BucketAlreadyExists</Code></Error>
```

#### 示例
##### 请求示例
```
PUT /new-bucket HTTP/1.1
Host: obs.eayun.com
Content-Length: 164
Accept-Encoding: gzip, deflate
x-amz-grant-read: id=zc-test-2
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Mon, 14 Dec 2015 04:22:10 GMT
Authorization: AWS BDNGQWVO6BCMTF5YTKKF:Ik5IWxif6+7iJRJ9MsKXCkufTGI=

<CreateBucketConfiguration xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <LocationConstraint>
    chengdu
  </LocationConstraint>
</CreateBucketConfiguration>
```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Cache-control: private
Date: Mon, 14 Dec 2015 04:22:17 GMT
Content-Type: application/xml


```
