## 删除对象

### 描述

删除服务器已经存在的对象。

### 请求信息:

#### 请求格式

```
DELETE /BucketName/ObjectName HTTP/1.1
Host: obs.eayun.com
Authorization: auth
Date: date
```

#### 请求消息头

|  消息头名称  |  描述  |  必选  |
|  :---  |  :---  |  :---  |
|  **versionId**  |  待删除对象的版本号  |  否  |

#### 请求参数

该请求中无请求参数
    
#### 请求消息元素

该请求中无请求消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
```

#### 响应消息头

请参考公共响应消息头

另外：

|  消息头名称  |  描述  |
|  :---  |  :---  |
|  **x-amz-delete-marker**  |  是否标记为删除；若无标记，则不显示  |
|  **x-amz-version-id**  |  对象的版本号；若对象无版本号，则不显示  |

#### 响应消息元素

该响应无消息元素

#### 错误响应消息

请参考错误响应描述

### 示例:

删除对象/kmnt/outkey-copy。

#### 请求实例

```
DELETE /outkey-copy HTTP/1.1
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:EyxjEsH0jSTPbxbIenQqipzNd6A=
Date: Tue, 24 Nov 2015 01:36:42 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 204 
Date: Tue, 24 Nov 2015 01:36:42 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph1; path=/
```