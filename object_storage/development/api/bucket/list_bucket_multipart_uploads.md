### 获取针对桶内对象多段上传的任务列表
#### 描述
获取当前正在进行的针对桶内对象的多段上传任务列表。

#### 请求消息
##### 请求格式
```
GET /{bucket}?uploads HTTP/1.1
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
| max-parts | 用于指定返回中包含多少条目的查询结果。 <br /> 返回的结果中包含的条目只可能少于指定的数值。如果符合查询条件的结果数目大于 `max-parts` 指定的数值，响应消息元素中会包含 `<IsTruncated>True</IsTruncated>` 。如果要获取更多的查询结果，可以再次发起请求，并指定 `key-marker` 和 `upload-id-marker` 参数。 <br /> 类型：字符串（数字） <br /> 默认值: 1000 | 否 |
| key-marker | 和 `upload-id-marker` 一起用于指定何处开始显示查询结果的标记。<br /> 如果没有指定 `upload-id-marker` ，则才查询结果列表中将从 `key-marker` 之后的对象开始。 <br /> 如果同时指定了 `upload-id-marker` ，则查询结果中和 `key-marker` 相同的对象，上传任务 ID 大于 `upload-id-marker` 的任务也会列出。 <br /> 类型：字符串 <br /> 默认值：无 | 否 |
| prefix | 限制查询结果中的对象名称必须以指定的字符串开头。 <br /> 类型：字符串 <br /> 默认值：无 | 否 |
| upload-id-marker | 和 `key-marker` 一起用于指定何处开始显示查询结果的标记。如果没有指定 `key-marker` ，则 `upload-id-marker` 将不起作用。如果同时指定了 `key-marker` ，则查询结果中和 `key-marker` 相同的对象，上传任务 ID 大于 `upload-id-marker` 的任务也会列出。 <br /> 类型：字符串 <br /> 默认值：无 | 否 |

#### 请求消息元素
该请求无请求消息元素

#### 响应消息
```
HTTP/1.1 {StatusCode}
Date: {Date}
Server: {Server}
Connection: close
Content-Type: application/xml

<?xml version="1.0" ?><ListMultipartUploadsResult><Bucket>{BucketName}</Bucket><KeyMarker>{KeyMarker}</KeyMarker><UploadIdMarker>{UploadIdMarker}</UploadIdMarker><NextKeyMarker>{NextKeyMarker}</NextKeyMarker><NextUploadIdMarker>{NextUploadIdMarker}</NextUploadIdMarker><MaxUploads>{MaxUploads}</MaxUploads><IsTruncated>{IsTrucated}</IsTruncated><Upload><Key>{ObjectId}</Key><UploadId>{UploadId}</UploadId><Initiator><ID>{InitiatorID}</ID><DisplayName>{InitiatorDisplayName}</DisplayName></Initiator><Owner><ID>{OwnerID}</ID><DisplayName><OwnerDisplayName}</DisplayName></Owner><StorageClass>{StorageClass}</StorageClass><Initiated>{InitiatedTime}</Initiated></Upload>...<Upload>...<Upload></ListMultipartUploadsResult>
```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
| 元素名称 | 描述 |
| :-- | :-- |
| ListMultipartUploadsResult | 响应消息数据中最上层数据结构，包含： `Bucket` ， `KeyMarker` ， `UploadIdMarker` ， `NextKeyMarker` ， `NextUploadIdMarker` ， `MaxUploads` ， `Delimiter` ， `Prefix` ，`CommonPrefixes` ， `IsTruncated` 。 <br /> 类型： XML 容器 <br /> 上层数据结构： 无 |
| Bucket | 上传任务针对对象所在的桶。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| KeyMarker | 说明响应消息中显示的查询结果从哪一个对象开始。由请求消息中的 `key-marker` 参数指定。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| UploadIdMarker | 说明响应消息中显示的查询结果从哪一个上传任务开始。由请求消息中的 `upload-id-marker` 参数指定。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| NextKeyMarker | 如果查询结果没有完全显示，可以使用 `NextKeyMarker` 作为下次查询请求的 `key-marker` 参数以获取之后的查询结果。查询结果是按照字典顺序排列的。 <br /> 类型：字符 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| NextUploadIdMarker | 如果查询结果没有完全显示，可以使用 `NextUploadIdMarker` 作为下次查询请求的 `upload-id-marker` 参数以获取之后的查询结果。查询结果是按照字典顺序排列的。 <br /> 类型：字符 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| MaxUploads | 响应消息中最多显示多少条查询结果。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| IsTruncated | 响应消息中是否显示了所有匹配查询条件的结果。如果请求消息中指定了 `max-parts` 参数，而查询结果大于该值，则将有部份结果不会显示。 <br /> 类型：布尔值 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| Upload | 响应消息中具体上传任务条目的上层 XML 容器，包含： `Key` ， `UploadId` ， `InitiatorOwner` ， `StorageClass` ， `Initiated` 。 <br /> 类型：XML 容器 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| Key | 上传任务所针对对象的名称。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| UploadId | 上传任务 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| Initiator | 上传任务的发起用户对应的 XML 容器。包含： `ID` 和 `DisplayName` 。 <br /> 类型： XML 容器 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| ID | 上传用户或者对象所有者的 ID 。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload.Initiator` 或者 `ListMultipartUploadsResult.Upload.Owner` |
| DisplayName | 上传用户或者对象所有者的显示名称。 <br /> 类型：字符串 <br /> 上层数据结构： `ListMultipartUploadsResult.Upload.Initiator` 或者 `ListMultipartUploadsResult.Upload.Owner` |
| Owner | 对象所有者对应的 XML 容器。包含 `ID` 和 `DisplayName` 。 <br /> 类型：XML 容器 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| StorageClass | 目前只有 `STANDARD` 类型。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| Initiated | 上传任务发起的日期和时间。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult.Upload` |
| Prefix | 查询条件中要求对象名称以此开头。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| Delimiter | 查询结果中名称以请求参数 `prefix` 开头一直到下一个匹配的 `delimiter` 为止都相同的对象在响应消息中将显示为同一条 `CommonPrefixes` 数据。这些结果因为显示为一条记录，所以只会占用`max-parts` 请求消息参数或者 `MaxUploads` 响应消息元素限制的 1 个。 <br /> 类型：字符串 <br /> 上层数据结构：`ListMultipartUploadsResult` |
| CommonPrefixes | 只有在请求消息中指定了 `delimiter` 参数，响应消息中才会包含 `CommonPrefixes` 。响应消息中的 `CommonPrefixes` 会包含所有以 `prefix` 开头并一直到下一个 `delimiter` 为止都相同的对象。举例来说，`prefix` 是 `eayun-public/` ，`delimiter` 是 `/` ，则对于对象 `eayun-public/docs/api.pdf` ，分组依据的共同前缀就是 `eayun-public/docs/` 。所有符合同一分组依据的对象在响应消息中显示为一条数据。关于响应数据显示结果的上限，参见 `max-parts` 请求消息参数或者 `MaxUploads` 响应消息元素。 <br /> 类型：字符串 <br /> 上层数据结构： `ListMultipartUploadsResult` |

##### 错误响应消息
请参考[错误响应描述](../error.md)

#### 示例
##### 请求示例
```
GET /new-bucket?uploads&max-parts=2 HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Mon, 30 Nov 2015 09:21:05 GMT
Authorization: AWS 9FSTXQR54VKR7VEM7GAJ:Ho1VzfDu4Tb5Ao8Gw9eaQwVocMo=


```

##### 响应示例
```
HTTP/1.1 200
Transfer-Encoding: chunked
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Cache-control: private
Date: Mon, 30 Nov 2015 09:20:55 GMT
Content-Type: application/xml

<?xml version="1.0" ?>
<ListMultipartUploadsResult>
  <Bucket>new-bucket</Bucket>
  <NextKeyMarker>test_2</NextKeyMarker>
  <NextUploadIdMarker>2/cFi1sQHNuXVWCF0ApPqOuNIlVr2gEv5</NextUploadIdMarker>
  <MaxUploads>2</MaxUploads>
  <IsTruncated>true</IsTruncated>

  <Upload>
    <Key>test_1</Key>
    <UploadId>2/0GZlbQxfMKFbEnvNLpyT6tYQ6s82sKc</UploadId>
    <Initiator>
      <ID>zc-test-1</ID>
      <DisplayName>Testing User for ZhaoChao</DisplayName>
    </Initiator>
    <Owner>
      <ID>zc-test-1</ID>
      <DisplayName>Testing User for ZhaoChao</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
    <Initiated>2015-11-30T09:13:07.000Z</Initiated>
  </Upload>
  <Upload>
    <Key>test_2</Key>
    <UploadId>2/cFi1sQHNuXVWCF0ApPqOuNIlVr2gEv5</UploadId>
    <Initiator>
      <ID>zc-test-1</ID>
      <DisplayName>Testing User for ZhaoChao</DisplayName>
    </Initiator>
    <Owner>
      <ID>zc-test-1</ID>
      <DisplayName>Testing User for ZhaoChao</DisplayName>
    </Owner>
    <StorageClass>STANDARD</StorageClass>
    <Initiated>2015-11-30T09:14:28.000Z</Initiated>
  </Upload>
</ListMultipartUploadsResult>
```
