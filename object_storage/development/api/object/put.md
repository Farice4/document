## 上传对象

### 描述

上传对象到指定的桶内，用户需要对该桶具有写权限。

### 请求信息:

#### 请求格式

```
PUT /BucketName/ObjectName HTTP/1.1
Host: obs.eayun.com
Content-Type: type
Content-Length: length
Authorization: auth
Date: date
<Optional Additional Header>

<object Content>
```

#### 请求消息头

|  消息头名称  |  描述  | 必选  |
|  :---  |  :---  |  :---  |
|  **Content-MD5**  |  指定对象的以Base64编码的MD5的hash值  |  否  |
|  **x-amz-meta-<...>**  |  用户自定义该对象的元数据，可用来管理对象  |   否   |
|  **x-amz-acl**  |  设置该对象的控制策略；<br>可选：private, public-read, public-read-write, authenticated-read  |   否   |

#### 请求参数

该请求中无请求参数
    
#### 请求消息元素

该请求中无请求消息元素，需要对象的数据
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Connection: close
Date: date
Content-Length: length
Content-Type: type
```

#### 响应消息头

|  消息头名称  |  描述  |
|  :---  |  :---  |
|  **x-amz-version-id**  |  若开启桶的多版本属性，则会返回对象的版本号  |

#### 响应消息元素

该响应消息无消息元素

#### 错误响应消息

请参考错误响应描述

### 示例:

上传本地文件key到桶kmnt内，改名为outkey，并设置自定义元数据x-amz-meta-test: test meta，控制策略x-amz-acl: private。

#### 请求实例

```
PUT /outkey HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:5pOGBUf1bvtGthZZgmFJRJmybwo=
Date: Thu, 19 Nov 2015 06:01:20 +0000
Content-Type: application/octet-stream
x-amz-meta-test: test meta
x-amz-acl: private
Content-Length: 62

File Content...
```

#### 响应实例

```
HTTP/1.1 200 
Date: Thu, 19 Nov 2015 06:01:20 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Accept-Ranges: bytes
ETag: "0fe43e64ffdf448e5b63294f09d1ec7a"
Content-Length: 0
Connection: close
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph2; path=/
Cache-control: private
```
