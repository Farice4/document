## 获取对象元数据

### 描述

获取对象的元数据，需要用户需要对该对象具有读权限。在开启多版本的情况下，默认获取的是最新版本对象的元数据。

### 请求信息:

#### 请求格式

```
HEAD /BucketName/ObjectName HTTP/1.1
Host: obs.eayun.com
Date: date
Authorization: auth
```

#### 请求消息头

| 消息头名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **Range**  |  获取对象元数据在Range范围内的内容；<br>格式Range: bytes={beginbyte}-{endbyte}；<br>如Range: bytes=0-4  |   否   |
|  **If-Modified-Since**  |  判断对象在该参数指定时间之后是否修改过，修改过则返回对象元数据内容，反之返>回304；  |   否   |
|  **If-Unmodified-Since**  |  判断对象在该参数时间之后是否修改过，没有修改过则返回对象元数据内容，反之>返回412；  |   否   |
|  **If-Match**  |  判断对象的Etag与该参数指定Etag是否相等，相等则返回对象元数据内容，反之返回412；  |   否   |
|  **If-None-Match**  |  判断对象的Etag与该参数指定Etag是否相等，不相等则返回对象元数据内容，反之返回304；  |   否   |

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **versionId**  |  指定获取对象的版本号；<br>如versionId=1  |   否   |
    
#### 请求消息元素

该请求中无请求消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Content-Type: type
Date: date
Content-Length: length
Etag: etag
Last-Modified: time
```

#### 响应消息头

| 消息头名字 | 描述 |
|  :---  |  :---  |
|  **x-amz-version-id**  |  对象的版本号，若没有开启版本，则没有该项  |
|  **x-amz-expiration**  |  对象的过期信息，若没有配置对象的过期时间，则没有该项  |
TODO：x-amz-website-redirect-location，CORS配置

#### 响应消息元素

该响应消息无消息元素

#### 错误响应消息

请参考错误响应描述

### 示例:

获取对象/kmnt/outkey的元数据。

#### 请求实例

```
HEAD /outkey HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:cboHkJUBgxwv7b331HKASGbOcIM=
Date: Thu, 19 Nov 2015 06:29:40 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 200 
Date: Thu, 19 Nov 2015 06:29:59 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Accept-Ranges: bytes
x-amz-meta-test: test meta
Content-Length: 62
Last-Modified: Thu, 19 Nov 2015 06:01:20 GMT
ETag: "0fe43e64ffdf448e5b63294f09d1ec7a"
Connection: close
Content-Type: application/octet-stream
Set-Cookie: RADOSGWLB=ceph3; path=/
Cache-control: private
```