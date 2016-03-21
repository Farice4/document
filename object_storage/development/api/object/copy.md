## 复制对象

### 描述

复制对象可以在一个桶内复制，也可复制到另一个桶内。

> ####注意
> 当复制的源对象所在桶开启了多版本，x-amz-copy-source指定的对象默认是最新版本的对象，也可以在x-amz-copy-source中指定对应的版本。

### 请求信息:

#### 请求格式

```
PUT /BucketName/DestinationObjectName HTTP/1.1
Host: obs.eayun.com
x-amz-copy-source: /SourceBucket/SourceObject
x-amz-metadata-directive: metadata_directive
x-amz-copy-source-if-match: etag
x-amz-copy-source-if-none-match: etag
x-amz-copy-source-if-unmodified-since: time_stamp
x-amz-copy-source-if-modified-since: time_stamp
Authorization: auth
Date: date
```

#### 请求消息头

| 消息头名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **x-amz-copy-source**  |  指定复制对象的源桶名和源对象；<br>如：/my-bucket/demo.pdf  |   是   |
|  **x-amz-acl**  |  设置复制的对象的控制策略；<br>可选：private, public-read, public-read-write, authenticated-read  |   否   |
|  **x-amz-metadata-directive**  |  设置复制的对象的元数据来源；<br>可选：COPY,REPLACE；<br>COPY代表复制源对象的元数据，<br>REPLACE代表使用请求消息中的的元数据；<br>默认：COPY  |   否   |  
|  **x-amz-copy-if-modified-since**  |  判断源对象在该参数指定时间之后是否修改过，修改过则进行复制操作，反之返回412；  |   否   |
|  **x-amz-copy-if-unmodified-since**  |  判断源对象在该参数时间之后是否修改过，没有修改过则进行复制操作，反之返回412；  |   否   |
|  **x-amz-copy-if-match**  |  判断源对象的Etag与该参数指定Etag是否相等，相等则进行复制操作，反之返回412；  |   否   |
|  **x-amz-copy-if-none-match**  |  判断源对象的Etag与该参数指定Etag是否相等，不相等则进行复制操作，反之返回412；  |   否   |

TODO:   x-amz-website-redirect-location
#### 请求参数

该请求中无请求参数
    
#### 请求消息元素

该请求中无请求消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Content-Type: type 
Date: date
Content-Length: length
Connection: close

<CopyObjectResult>
<LastModified>2015-11-19T06:10:09.000Z</LastModified>
</CopyObjectResult>
```

#### 响应消息头

请参考公共响应消息头

另外：

|  消息头名称  |  描述  |
|  :---  |  :---  |
|  **x-amz-copy-source-version-id**  |  源对象的版本号  |
|  **x-amz-version-id**  |  目标对象的版本号  |

#### 响应消息元素

|  元素名称  |  描述  |
|  :---:  |  :---:  |
|  CopyObjectResult  |  复制操作结果的Container;<br>类型：xml  |
|  LastModified  |  对象上次修改的时间；<br>类型：字符串。  |
|  ETag  |  新对象的ETag值；<br>类型：字符串  |

#### 错误响应消息

请参考错误响应描述

### 示例:

复制/kmnt/outkey为/kmnt/outkey-copy，并设置元数据从源对象复制过来。

#### 请求实例

```
PUT /outkey-copy HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:eim44FfFXX4iwQH9Xe+4gC++klk=
Date: Thu, 19 Nov 2015 06:10:09 +0000
Content-Type: application/octet-stream
x-amz-copy-source: /kmnt/outkey
x-amz-metadata-directive: COPY
x-amz-acl: private
Content-Length: 62
```

#### 响应实例

```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 07:01:16 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: binary/octet-stream
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private

<CopyObjectResult>
<LastModified>2015-11-20T07:01:16.000Z</LastModified>
</CopyObjectResult>
```