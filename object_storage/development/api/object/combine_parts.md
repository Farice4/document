## 合并段

### 描述

当段都上传完成，就可以执行合并操作，让各段在服务端合并成一个完整可用对象。
合并的对象的Etag生成公式：MD5(M1M2...Mn)-N,Mn为第段n的MD5,N为总的段数。  
合并操作若没有使用所有的段，那么合成的对象将会少了一些内容，没有使用的段将会被自动删除。

### 请求信息:

#### 请求格式

```
POST /BucketName/ObjectName?uploadId=uploadID HTTP/1.1
Host: obs.eayun.com
Date: date
Content-Length: length
Authorization: auth

<CompleteMultipartUpload>
<Part>
<PartNumber>partNum1</PartNumber>
<ETag>etag1</ETag>
</Part>
...
</CompleteMultipartUpload>
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **uploadId**  |  多段上传任务ID  |   是   |

#### 请求消息元素

| 元素名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **CompleteMultipartUpload**  |  part列举的标签  |   是   |
|  **Part**  |  每个段的信息的标签  |   是   |
|  **PartNumber**  |  该段的段号  |   是   |
|  **ETag**  |  该任务的Etag  |   是   |
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
Connection: state

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Location>Bucket.obs.eayun.com</Location>
<Bucket>BucketName</Bucket>
<Key>ObjectName</Key>
<ETag>ETag</ETag>
</CompleteMultipartUploadResult>
```

#### 响应消息头

| 参数名字 | 描述 |
|  :---  |  :---  |
|  **x-amz-version-id**  |  合并得到的对象的版本号  |

#### 响应消息元素

| 元素名字 | 描述 |
|  :---  |  :---  |
|  **ListMultipartUploadResult**  |  part列举的标签  |
|  **Bucket**  |  对象所属的桶  |
|  **Key**  |  对象的名字  |
|  **UploadId**  |  多段上传任务ID  |
|  **Initiator**  |  该任务的所属者  |
|  **Owner**  |  同Initiator  |
|  **ID**  |  该任务的所属者的ID  |
|  **DisplayName**  |  该任务的所属者的名字  |
|  **PartNumberMarker**  |  part的起始段号  |
|  **NextPartNumberMarker**  |  此次列出操作的最后一个段号，当未列完的时候，可以作为下一次请求中的PartNumberMarker  |
|  **IsTruncated**  |  表示是否被截断，<br>true表示本次没有返回全部结果；<br>false表示本次已经返回了全部结果  |
|  **Part**  |  每个段的信息的标签  |
|  **PartNumber**  |  该段的段号  |
|  **LastModified**  |  该段的上传时间  |
|  **ETag**  |  该任务的Etag  |
|  **Size**  |  该任务的size  |

#### 错误响应消息

请参考错误响应描述

另外：

1. 当桶不存在时，返回404 Not Found，错误码为NoSuchBucket。
2. 当多段上传任务不存在时，返回404 Not Found，错误码为NoSuchUpload。
3. 当段大小超过5G，则返回错误400 Bad Request。
4. 当段序号超过范围[1,10000]，则返回错误400 Bad Request。

### 示例:

合并任务ID为2/WDkVkmtfKYOICFXuvKv_BcC2q4Ps1MX的上传的段，需要提供合并段的信息；最终响应返回合并成功的对象的信息。

#### 请求实例

```
POST /part?uploadId=2/WDkVkmtfKYOICFXuvKv_BcC2q4Ps1MX HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:cDD9p9uQxPQbkX/MgIsPcjC2Boo=
Date: Fri, 20 Nov 2015 06:37:34 +0000
Content-Type: application/octet-stream
Content-Length: 228

<CompleteMultipartUpload>
<Part>
<PartNumber>1</PartNumber>
<ETag>56730d3091a764d5f8b38feeef0bfcef</ETag>
</Part>
<Part>
<PartNumber>2</PartNumber>
<ETag>56730d3091a764d5f8b38feeef0bfcef</ETag>
</Part>
</CompleteMultipartUpload>
```

#### 响应实例

```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 06:37:56 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph3; path=/

<?xml version="1.0" encoding="UTF-8"?>
<CompleteMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Location>kmnt.obs.eayun.com</Location>
<Bucket>kmnt</Bucket>
<Key>part</Key>
<ETag>b3f4f3f0c47a71afa5c38fc85f7136c9-2</ETag>
</CompleteMultipartUploadResult>
```
