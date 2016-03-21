## 多段上传

### 描述

当初始化一个多段上传任务之后，接下来需要将已经划分好的段进行上传。上传顺序对合并没有影响，可以实现并发上传。  
对于段有一些要求：每个段大小是5M-5G；最后一个段大小是0-5G；段号范围是1-10000；  
上传的段在为合并之前不能被当作对象操作，即不能下载查看，但这些段是已经占用了存储资源。

### 请求信息:

#### 请求格式

```
PUT /BucketName/ObjectName?partNumber=partNum&uploadId=uploadID  HTTP/1.1 
Host: obs.eayun.com
Date: date
Content-Length: Size 
Authorization: Signature
Content-MD5：md5

<object Content>
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **partNumber**  |  上传的段的段号；<br>格式：整数  |   是   |
|  **uploadId**  |  多段上传任务的ID  |   是   |

#### 请求消息元素

该请求无消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date 
Content-Length: length 
Connection: status 

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Bucket>BucketName</Bucket> 
<Key>ObjectName</Key> 
<UploadId>uploadID</UploadId>
</InitiateMultipartUploadResult> 
```

#### 响应消息头

请参考公共响应消息头

#### 响应消息元素

该响应没有消息元素

#### 错误响应消息

请参考错误响应描述

另外：

1. 当桶不存在时，返回404 Not Found，错误码为NoSuchBucket。
2. 当多段上传任务不存在时，返回404 Not Found，错误码为NoSuchUpload。
3. 当段大小超过5G，则返回错误400 Bad Request。
4. 当段序号超过范围[1,10000]，则返回错误400 Bad Request。

### 示例:

上传本地文件key作为对象/kmnt/part的第一段。

#### 请求实例

```
PUT /part?partNumber=1&uploadId=2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:BUi4e9Go1dMme6RtAfw4HksVM60=
Date: Fri, 20 Nov 2015 05:35:58 +0000
Content-Type: application/octet-stream
Content-Length: 21

File Content...
```

#### 响应实例

```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 05:36:20 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Accept-Ranges: bytes
ETag: "418a802c5fd356bdc0a14563cd91b90f"
Content-Length: 0
Connection: close
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph3; path=/
Cache-control: private
```
