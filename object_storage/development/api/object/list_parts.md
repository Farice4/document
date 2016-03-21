## 列出所有段

### 描述

根据多段上传任务的ID列出所有已经上传的段的信息，也是接下来段合并需要的数据。

### 请求信息:

#### 请求格式

```
GET /BucketName/ObjectName?uploadId=uploadid&max-parts=max&part-number-marker=marker HTTP/1.1
Host: Host Server
Date: date
Authorization: auth
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **uploadId**  |  多段上传任务ID  |   是   |
|  **max-parts**  |  限定列出的part最大数目  |   否   |
|  **part-number-marker**  |  限定列出大于指定的段号的段  |   否   |

#### 请求消息元素

该请求无消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
Content-Length: length
Connection: state
 
<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Bucket>BucketName</Bucket>
<Key>object</Key>
<UploadId>uploadid</UploadId>
<Initiator>
<ID>initiatorid</ID> 
<DisplayName>displayname</DisplayName>
</Initiator>
<Owner>
<ID>ownerid</ID>
<DisplayName>ownername</DisplayName>
</Owner>
<StorageClass>STANDARD</StorageClass>
<PartNumberMarker>partNmebermarker</PartNumberMarker>
<NextPartNumberMarker>nextpartnumbermarker</NextPartNumberMarker>
<MaxParts>2</MaxParts>
<IsTruncated>true</IsTruncated>
<Part>
<PartNumber>partnumber</PartNumber>
<LastModified>modifieddate</LastModified>
<ETag>etag</ETag>
<Size>size</Size>
</Part>
 ... 
</ListPartsResult>
```

#### 响应消息头

请参考公共响应消息头

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

列出任务ID为2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP的已经上传成功的段信息，根据响应可以看出有2段。

#### 请求实例

```
GET /part?uploadId=2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:Abx3Cy6Xe6KpsZUtZ6Yg7AwwQoE=
Date: Fri, 20 Nov 2015 05:39:40 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 05:40:02 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph3; path=/
Cache-control: private

<?xml version="1.0" encoding="UTF-8"?>
<ListMultipartUploadResultxmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Bucket>kmnt</Bucket>
<Key>part</Key>
<UploadId>2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP</UploadId>
<StorageClass>STANDARD</StorageClass>
<PartNumberMarker>0</PartNumberMarker>
<NextPartNumberMarker>3</NextPartNumberMarker>
<MaxParts>1000</MaxParts>
<IsTruncated>false</IsTruncated>
<Owner>
<ID>testuser</ID>
<DisplayName>testuser</DisplayName>
</Owner>
<Part>
<LastModified>2015-11-20T05:36:21.000Z</LastModified>
<PartNumber>1</PartNumber>
<ETag>418a802c5fd356bdc0a14563cd91b90f</ETag>
<Size>21</Size>
</Part>
<Part>
<LastModified>2015-11-20T05:37:34.000Z</LastModified>
<PartNumber>2</PartNumber>
<ETag>1612e3349d93aeff3a010bc713852f67</ETag>
<Size>28</Size>
</Part>
</ListMultipartUploadResult>
```