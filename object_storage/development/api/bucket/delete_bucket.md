### 删除桶
#### 描述
删除指定的桶。

#### 请求信息
##### 请求格式
```
DELETE /{bucket} HTTP/1.1
Host: obs.eayun.com

Authorization: AWS {access-key}:{hash-of-header-and-secret}
```

#### 请求消息头
##### 公共请求消息头
请参考[公共消息头](../header.md)

#### 请求消息参数
该请求无请求消息参数

#### 请求消息元素
该请求无请求消息元素

#### 响应消息
##### 响应格式
```
HTTP/1.1 {StatusCode}
Date: {Date}
Server: {Server}
Connection: close
Content-Type: application/xml


```

##### 响应消息头
请参考[公共响应消息头](../header.md)

##### 响应消息元素
该请求无任何响应消息元素。

#### 示例
##### 请求示例
```
DELETE /new-bucket HTTP/1.1
Host: obs.eayun.com
Content-Length: 0
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.8.1
Connection: keep-alive
date: Mon, 23 Nov 2015 11:49:14 GMT
Authorization: AWS BDNGQWVO6BCMTF5YTKKF:uWvpzu48mtkiDlr+EiemNpXoz9o=
```

##### 响应示例
```
HTTP/1.1 204
Date: Mon, 23 Nov 2015 11:49:14 GMT
Connection: close
Content-Type: application/xml
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips


```
