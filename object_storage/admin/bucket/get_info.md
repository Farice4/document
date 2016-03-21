## 获取桶的信息
### 描述
获取桶的详细信息, 如果指定了 uid(无论指定 bucket 与否), 返回所有属于该用户的桶的名字. 如果只指定了 bucket, 那么只返回该 bucket 的相信信息, 如果 uid 和bucket 都不指定, 返回所有用户的 bucket 名字

### 请求信息:
#### 请求格式
```
GET /admin/bucket?format=json HTTP/1.1
Host: obs.eayun.com
date: Date
Authorization: auth
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
// TODO: stats 参数好像有 bug, 指定 false 也返回所有
| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **format** | 描述: 服务器返回内容的格式 <br>类型: 字符串<br>默认值: json<br>示例: json| 否 |
| **bucket** | 描述: 查询的bucket名字 <br>类型: 字符串 <br>默认值: 无 <br>示例: lucy| 否 |
| **uid** | 描述: 用户 ID<br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 否 |
| **stats** | 描述: bucket 的统计信息 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-19 16:00:00| 否 |

#### 请求消息元素
该请求中无请求消息元素

### 响应信息:
#### 响应格式
```
HTTP/1.1 StatusCode
Date: Date
Server: Server
Connection: close
Content-Type: Type

[BucketInfo]
```

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **name** | bucket 名字<br>类型: 字符串|
| **pool** | bucket 所在的 pool 名<br>类型:字符串|
| **owner** | bucket 的拥有者<br>类型: 字符串|
| **id** | bucket 的id<br>类型: 字符串|
| **marker** | 内部标记的 bucket tag<br>类型: 字符串|
| **usage** | bucket 的使用信息<br>类型: json 对象|
| **index_pool** |  bucket 所在的 index pool<br>类型: 字符串|
| **mtime** | 最后修改时间 <br>类型: 整型|
| **ver** | bucket 的版本号<br>类型: 整形|

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/bucket?format=json&uid=lucy&stats&bucket=lucy HTTP/1.1
Host: obs.eayun.com:9090
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Wed, 25 Nov 2015 02:29:10 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:6PQHxiqzLU9BGQRX9t4EglMNVBk=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Wed, 25 Nov 2015 02:29:10 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph1; path=/
Cache-control: private

[
    {
        "bucket": "lucy",
        "bucket_quota": {
            "enabled": false,
            "max_objects": -1,
            "max_size_kb": -1
        },
        "id": "center-1.4570.2",
        "index_pool": ".center-1.rgw.buckets.index",
        "marker": "center-1.4570.2",
        "master_ver": 0,
        "max_marker": "00000000022.23.3",
        "mtime": 1447913573,
        "owner": "lucy",
        "pool": ".center-1.rgw.buckets",
        "usage": {
            "rgw.main": {
                "num_objects": 9,
                "size_kb": 32437,
                "size_kb_actual": 32456
            }
        },
        "ver": 23
    },
    {
        "bucket": "lucy1",
        "bucket_quota": {
            "enabled": false,
            "max_objects": -1,
            "max_size_kb": -1
        },
        "id": "center-1.4683.2",
        "index_pool": ".center-1.rgw.buckets.index",
        "marker": "center-1.4683.2",
        "master_ver": 0,
        "max_marker": "00000000002.139.3",
        "mtime": 1447919155,
        "owner": "lucy",
        "pool": ".center-1.rgw.buckets",
        "usage": {
            "rgw.main": {
                "num_objects": 1,
                "size_kb": 7447,
                "size_kb_actual": 7448
            }
        },
        "ver": 3
    },
    {
        "bucket": "lucy2",
        "bucket_quota": {
            "enabled": false,
            "max_objects": -1,
            "max_size_kb": -1
        },
        "id": "center-1.4680.2",
        "index_pool": ".center-1.rgw.buckets.index",
        "marker": "center-1.4680.2",
        "master_ver": 0,
        "max_marker": "00000000042.118.3",
        "mtime": 1447991012,
        "owner": "lucy",
        "pool": ".center-1.rgw.buckets",
        "usage": {
            "rgw.main": {
                "num_objects": 2,
                "size_kb": 11841,
                "size_kb_actual": 11844
            },
            "rgw.multimeta": {
                "num_objects": 0,
                "size_kb": 0,
                "size_kb_actual": 0
            }
        },
        "ver": 43
    }
]
```

> ###### 注意
> 为了方便显示, 返回的 json 字符串做了排版
