### 获取桶列表
#### 描述
查看用户已经创建的桶列表。

#### 请求信息
##### 请求格式
```
GET / HTTP/1.1
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
Connection: close
Content-Type: application/xml

<?xml version="1.0" ?><ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Owner><ID>{BucketOwnerID}</ID><DisplayName>{BucketOwnerDisplayName}</DisplayName></Owner><Buckets><Bucket><Name>{BucketName}</Name><CreationDate>{BucketCreationDate}</CreationDate></Bucket>...</Buckets></ListAllMyBucketsResult>
```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
| 元素名称 | 描述 |
| :-- | :-- |
| Bucket | 查询结果中每一个桶对应的 XML 容器。 <br /> 类型：XML 容器，包含 `Name` 和 `CreationDate` 元素。 <br /> 上层数据结构：`ListAllMyBucketsResult.Buckets` |
| Buckets | 包含所有桶信息的 XML 容器。 <br /> 类型：XML 容器，包含 `Bucket` 元素。 <br /> 上层数据结构： `ListAllMyBucketsResult` |
| CreationDate | 桶的创建时间。 <br /> 类型：日期时间，格式为： `yyyy-mm-ddThh:mm:ss.timezone` 。 <br /> 上层数据结构：`ListAllMyBucketsResult.Buckets.Bucket` |
| DisplayName | 桶所有者的显示名称 <br /> 类型：字符串 <br /> 上层数据结构：`ListAllMyBucketsResult.Owner` |
| ID | 桶所有者的 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`ListAllMyBucketsResult.Owner` |
| ListAllMyBucketsResult | 响应消息的最上层 XML 容器。 <br /> 类型：XML 容器，包含 `Owner` 和 `Bucket` 元素。 <br /> 上层数据结构：无 |
| Name | 桶名称。 <br /> 类型：字符串 <br /> 上层数据结构：`ListAllMyBucketsResult.Buckets.Bucket` |
| Owner | 桶的所有者信息的 XML 容器。 <br /> 类型：XML 容器，包含 `ID` 和 `DisplayName` 元素。 <br /> 上层数据结构：`ListAllMyBucketsResult` |

##### 错误响应消息
请参考[错误响应描述](../error.md)

#### 示例
##### 请求示例
```
GET / HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Wed, 09 Dec 2015 06:02:23 GMT
Authorization: AWS BDNGQWVO6BCMTF5YTKKF:N/76z7VJNv75crU+b009OiwIK+4=
```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Cache-control: private
Date: Wed, 09 Dec 2015 06:02:30 GMT
Content-Type: application/xml

<?xml version="1.0" ?>
<ListAllMyBucketsResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Owner>
        <ID>zc-test-1</ID>
        <DisplayName>Testing Accout Created by ZhaoChao</DisplayName>
    </Owner>
    <Buckets>
        <Bucket>
            <Name>new-bucket</Name>
            <CreationDate>2015-11-24T03:11:32.000Z</CreationDate>
        </Bucket>
    </Buckets>
</ListAllMyBucketsResult>
```
