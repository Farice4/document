## 初始化多段上传

### 描述

上传对象可以选择将对象分为多段分别进行上传，互不干扰，最后再进行合并。  
在上传操作之前需要初始化一个多段上传的任务，这个任务包含任务ID以及初始化对象的少量信息(对象名字等)。后面的上传等一系列操作都需要携带这个任务的一些信息。

### 请求信息:

#### 请求格式

```
POST /BucketName/ObjectName?uploads  HTTP/1.1 
Host: obs.eayun.com 
Date: date 
Authorization: auth  
```

#### 请求消息头

参考公共请求消息头

TODO：x-amz-website-redirect-location

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **uploads**  |  指定为多段上传  |   是   |
    
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

| 元素名字 | 描述 |
|  :---  |  :---  |
|  **InitiateMultipartUploadResult**  |  指多端上传任务的标签  |
|  **Bucket**  |  多段上传对象所属的桶  |
|  **Key**  |  多段上传对象的名字  |
|  **UploadId**  |  多段上传任务的ID，后面进行上传时需要此ID  |

#### 错误响应消息

请参考错误响应描述

另外：

1. 如果AccessKey或签名无效，OBS返回403 Forbidden，错误码为AccessDenied。
2. 如果桶不存在，OBS返回404 Not Found，错误码为NoSuchBucket。
3. 检查用户是否具有指定桶的写权限，如果没有权限，OBS返回403 Forbidden，错误码为AccessDenied。

### 示例:

初始化一个多段上传任务，最终上传的对象是/kmnt/partup。

#### 请求实例

```
POST /partup?uploads HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:26ZTk/WviI7BL62mA+0ALsngMZ0=
Date: Fri, 20 Nov 2015 03:20:23 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 03:20:23 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph1; path=/

<?xml version="1.0" encoding="UTF-8"?>
<InitiateMultipartUploadResult xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
<Bucket>kmnt</Bucket>
<Key>partup</Key>
<UploadId>2/AzrHB9uqM-t2KUmP0UV9vBfIu_RWwIA</UploadId>
</InitiateMultipartUploadResult>
```