## 删除多段上传任务

### 描述

当不进行多段合并操作的时候，已经上传的段实质上会占用的存储空间，最好将这个多段上传任务删除。

### 请求信息:

#### 请求格式

```
DELETE /BucketName/ObjectName?uploadId=uplaodID HTTP/1.1
Host: obs.eayun.com
Date: date
Authorization: auth
```

#### 请求消息头

参考公共请求消息头

#### 请求参数

| 参数名字 | 描述 | 必选 |
|  :---  |  :---  |  :---  |
|  **uploadId**  |  多段上传任务ID  |   是   |

#### 请求消息元素

该请求无消息元素
    
### 响应信息:

#### 响应格式

```
HTTP/1.1 status_code
Date: date
```

#### 响应消息头

请参考公共消息头

#### 响应消息元素

该响应无消息元素

#### 错误响应消息

请参考错误响应描述

另外：

1. 当桶不存在时，返回404 Not Found，错误码为NoSuchBucket。
2. 取消任务成功，返回204 No Content。

### 示例:

删除任务ID为2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP的上传任务。

#### 请求实例

```
DELETE /part?uploadId=2/MRooGdwcr8xmI3t9F0Htxf95FeWW1WP HTTP/1.1
cOsof3jz5MxMzXR3vnPWTH3E+cw=
Host: kmnt.obs.eayun.com
User-Agent: curl/7.43.0
Accept: */*
Authorization: AWS G7HGAZI01NOYBNWQ4EJD:cOsof3jz5MxMzXR3vnPWTH3E+cw=
Date: Fri, 20 Nov 2015 06:14:42 +0000
Content-Type: application/octet-stream
```

#### 响应实例

```
HTTP/1.1 204 
Date: Fri, 20 Nov 2015 06:14:42 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Content-Type: application/xml
Set-Cookie: RADOSGWLB=ceph2; path=/
```