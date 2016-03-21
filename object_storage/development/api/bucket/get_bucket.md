### 获取桶中对象列表
#### 描述
查看桶中包含哪些对象。

#### 请求信息
##### 请求格式
```
GET /{bucket} HTTP/1.1
Host: obs.eayun.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

#### 请求消息头
##### 公共请求消息头
请参考[公共消息头](../header.md)

#### 请求消息参数
| 参数名称 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| delimiter | 用于将查询结果中的对象分组的分隔符。 <br /> 如果指定了 `prefix` 参数，所有以 `prefix` 中开头并包含 `delimiter` 的对象, 将根据从 `prefix` 开始到下一个 `delimiter` 为止的字符串为依据分组，同一个分组在查询结果中显示为一个 `CommonPrefixes` 结果；如果没有指定 `prefix` 参数，查询结果的分组依据为对象名称开始一直到下一个 `delimiter` 组成的字符串。`CommonPrefixes` 结果中不会显示具对的对象名称。 <br /> 类型：字符串（一个字符） <br /> 默认值：无 | 否 |
| marker | 用于指定何处开始显示查询结果的标记。易云云存储按照字典顺序显示查询结果，当次显示的查询结从 `marker` 指定的对象开始。 <br /> 类型：字符串 <br /> 默认值：无 | 否 |
| max-keys | 用于指定返回中包含多少条目的查询结果。 <br /> 返回的结果中包含的条目只可能少于指定的数值。如果符合查询条件的结果数目大于 `max-keys` 指定的数值，响应消息元素中会包含 `<IsTruncated>True</IsTruncated>` 。如果要获取更多的查询结果，可以再次发起请求，并指定 `marker` 参数。 <br /> 类型：字符串（数字） <br /> 默认值: 1000 | 否 |
| prefix | 限制查询结果中的对象名称必须以指定的字符串开头。 <br /> 类型：字符串 <br /> 默认值：无 | 否 |

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

<?xml version="1.0" ?><ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/"><Name>{BucketName}</Name><Prefix>{Prefix}</Prefix><Marker/>{Marker}<MaxKeys>{MaxKeys}</MaxKeys><IsTruncated>{IsTruncated}</IsTruncated><Contents><Key>{ObjectName}</Key><LastModified>{ObjectMTime}</LastModified><ETag>{ObjectETag}</ETag><Size>{ObjectSize}</Size><StorageClass>STANDARD</StorageClass><Owner><ID>{ObjectOwnerID}</ID><DisplayName>{ObjectOwnerDisplayName}</DisplayName></Owner></Contents><Contents>...</Contents>...</ListBucketResult>```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
| 元素名称 | 描述 |
| :-- | :-- |
| Contents | 符合查询条件的每一个对象的元数据。 <br /> 类型：XML 元数据 <br /> 上层数据结构：`ListBucketResult` |
| CommonPrefixes | 只有在请求消息中指定了 `delimiter` 参数，响应消息中才会包含 `CommonPrefixes` 。响应消息中的 `CommonPrefixes` 会包含所有以 `prefix` 开头并一直到下一个 `delimiter` 为止都相同的对象。举例来说，`prefix` 是 `eayun-public/` ，`delimiter` 是 `/` ，则对于对象 `eayun-public/docs/api.pdf` ，分组依据的共同前缀就是 `eayun-public/docs/` 。所有符合同一分组依据的对象在响应消息中显示为一条数据。关于响应数据显示结果的上限，参见 `max-keys` 请求消息参数或者 `MaxKeys` 响应消息元素。 <br /> 类型：字符串 <br /> 上层数据结构： `ListBucketResult` |
| Delimiter | 查询结果中名称以请求参数 `prefix` 开头一直到下一个匹配的 `delimiter` 为止都相同的对象在响应消息中将显示为同一条 `CommonPrefixes` 数据。这些结果因为显示为一条记录，所以只会占用`max-keys` 请求消息参数或者 `MaxKeys` 响应消息元素限制的 1 个。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult` |
| DisplayName | 对象所有者的显示名称 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents.Owner` |
| ETag | 该对象对应的 MD5 校验码。 `ETag` 的变化只说明对象包含的数据发生了变化，不反应元素的更改。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents` |
| ID | 对象所有者的 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents.Owner` |
| IsTruncated | 响应消息中是否显示了所有匹配查询条件的结果。如果请求消息中指定了 `max-keys` 参数，而查询结果大于该值，则将有部份结果不会显示。 <br /> 类型：布尔值 <br /> 上层数据结构：`ListBucketResult` |
| Key | 对象的名称。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents` |
| LastModified | 对象最后一次被修改的时间。 <br /> 类型：日期时间 <br /> 上层数据结构：`ListBucketResult.Contents` |
| Marker | 说明响应消息中显示的查询结果从哪一个对象开始。由请求消息中的 `marker` 参数指定。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult` |
| MaxKeys | 响应消息中最多显示多少条查询结果。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult` |
| Name | 桶名称。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult` |
| NextMarker | 如果查询结果没有完全显示，可以使用 `NextMarker` 作为下次查询请求的 `marker` 参数以获取之后的查询结果。查询结果是按照字典顺序排列的。 <br /> 类型：字符 <br /> 上层数据结构：`ListBucketResult` |
| Owner | 桶的所有者。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents` 或者 `ListBucketResult.CommonPrefixes` |
| Prefix | 查询条件中要求对象名称以此开头。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents` |
| Size | 对象的大小。 <br /> 类型：字符串（数字，单位：字节） <br /> 上层数据结构：`ListBucketResult.Contents` |
| StorageClass | 目前只有 `STANDARD` 类型。 <br /> 类型：字符串 <br /> 上层数据结构：`ListBucketResult.Contents` |

##### 错误响应消息
请参考[错误响应描述](../error.md)

#### 示例
##### 请求示例
```
GET /new-bucket?max-keys=2 HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Fri, 27 Nov 2015 04:14:15 GMT
Authorization: AWS BDNGQWVO6BCMTF5YTKKF:3+lBwr5BF9LzGqQgiJZa/ER5cHY=
```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Cache-control: private
Date: Fri, 27 Nov 2015 04:14:15 GMT
Content-Type: application/xml

<?xml version="1.0" ?>
<ListBucketResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
    <Name>new-bucket</Name>
    <Prefix/>
    <Marker/>
    <MaxKeys>2</MaxKeys>
    <IsTruncated>true</IsTruncated>
    <Contents>
        <Key>eayun-pics-2/Eayun-48.png</Key>
        <LastModified>2015-11-24T04:01:57.000Z</LastModified>
        <ETag>&quot;239bb1c966fcb4b462b67854d6e6e56e&quot;</ETag>
        <Size>3249</Size>
        <StorageClass>STANDARD</StorageClass>
        <Owner>
            <ID>zc-test-1</ID>
            <DisplayName>Testing Accout Created by ZhaoChao</DisplayName>
        </Owner>
    </Contents>
    <Contents>
        <Key>eayun-pics-2/new-dir/Eayun-64.png</Key>
        <LastModified>2015-11-24T09:52:17.000Z</LastModified>
        <ETag>&quot;c374358c670b3de37a1074716e9e1092&quot;</ETag>
        <Size>3407</Size>
        <StorageClass>STANDARD</StorageClass>
        <Owner>
            <ID>zc-test-1</ID>
            <DisplayName>Testing Accout Created by ZhaoChao</DisplayName>
        </Owner>
    </Contents>
</ListBucketResult>
```
