## 获取使用量信息
### 描述
获取相应的使用量信息

### 请求信息:
#### 请求格式
```
GET /admin/user?format=json HTTP/1.1
Host: obs.eayun.com
date: Date
Authorization: auth
```

#### 请求消息头
使用公共请求消息头，请参考[公共消息头](../header.md)

#### 请求参数
| 参数名字 | 描述 | 是否必选 |
| :-- | :-- | :-- |
| **format** | 描述: 服务器返回内容的格式 <br>类型: 字符串<br>默认值: json<br>示例: json| 否 |
| **uid** | 描述: 用户 ID, 如果没有指定用户 ID, 查询所有用户的使用量 <br>类型: 字符串 <br>默认值: 无 <br>示例:test_user | 否 |
| **start** | 描述: 查询使用量的开始日期和时间 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-10 16:00:00| 否 |
| **end** | 描述: 查询使用量的结束日期和时间 <br>类型: 字符串 <br>默认值: 无 <br>示例: 2015-11-19 16:00:00| 否 |
| **show-entries** | 描述: 是否显示 entries 信息<br>类型: Boolean <br>默认值: True <br>示例: False| 否 |
| **show-summary** | 描述: 是否显示 summary 信息<br>类型: Boolean <br>默认值: True <br>示例: False| 否 |

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

{
    "entries": [
        {
            "buckets": [
                {
                    "bucket": "",
                    "categories": [
                        {
                            "bytes_received": 0,
                            "bytes_sent": 428,
                            "category": "list_buckets",
                            "ops": 2,
                            "successful_ops": 2
                        }
                    ],
                    "epoch": 1447912800,
                    "time": "2015-11-19 06:00:00.000000Z"
                }
            ],
            "owner": "anonymous"
        },
        {
            "buckets": [
                {
                    "bucket": "",
                    "categories": [
                        {
                            "bytes_received": 0,
                            "bytes_sent": 900,
                            "category": "list_buckets",
                            "ops": 3,
                            "successful_ops": 3
                        }
                    ],
                    "epoch": 1447887600,
                    "time": "2015-11-18 23:00:00.000000Z"
                },
                {
                    "bucket": "lucy",
                    "categories": [
                        {
                            "bytes_received": 19750327,
                            "bytes_sent": 0,
                            "category": "put_obj",
                            "ops": 4,
                            "successful_ops": 4
                        }
                    ],
                    "epoch": 1447887600,
                    "time": "2015-11-18 23:00:00.000000Z"
                },
                {
                    "bucket": "lucy1",
                    "categories": [
                        {
                            "bytes_received": 0,
                            "bytes_sent": 0,
                            "category": "create_bucket",
                            "ops": 1,
                            "successful_ops": 1
                        },
                        {
                            "bytes_received": 7625710,
                            "bytes_sent": 0,
                            "category": "put_obj",
                            "ops": 1,
                            "successful_ops": 1
                        }
                    ],
                    "epoch": 1447887600,
                    "time": "2015-11-18 23:00:00.000000Z"
                },
            ],
            "owner": "lucy"
        },
    ],
    "summary": [
        {
            "categories": [
                {
                    "bytes_received": 0,
                    "bytes_sent": 428,
                    "category": "list_buckets",
                    "ops": 2,
                    "successful_ops": 2
                }
            ],
            "total": {
                "bytes_received": 0,
                "bytes_sent": 428,
                "ops": 2,
                "successful_ops": 2
            },
            "user": "anonymous"
        },
        {
            "categories": [
                {
                    "bytes_received": 0,
                    "bytes_sent": 0,
                    "category": "create_bucket",
                    "ops": 2,
                    "successful_ops": 2
                },
                {
                    "bytes_received": 0,
                    "bytes_sent": 599,
                    "category": "get_obj",
                    "ops": 1,
                    "successful_ops": 1
                },
                {
                    "bytes_received": 0,
                    "bytes_sent": 900,
                    "category": "list_buckets",
                    "ops": 3,
                    "successful_ops": 3
                },
                {
                    "bytes_received": 27376636,
                    "bytes_sent": 0,
                    "category": "put_obj",
                    "ops": 6,
                    "successful_ops": 6
                }
            ],
            "total": {
                "bytes_received": 27376636,
                "bytes_sent": 1499,
                "ops": 12,
                "successful_ops": 12
            },
            "user": "lucy"
        },
    ]
}
```

> ###### 注意
> 为了方便显示, 返回的 json 字符串做了排版

#### 响应消息头
请参考[公共响应消息头](../header.md)

#### 响应消息元素
| 参数名字 | 描述 |
| :-- | :-- |
| **entries** | 查询用户的条目信息, 每个用户一个数组成员<br>类型:数组 |
| **owner** | bucket 的拥有者<br>类型: 字符串|
| **buckets** | buckets 的信息<br>类型: 数组|
| **bucket** | bucket 名字<br>类型: 字符串|
| **time** | TODO<br>类型: 字符串|
| **epoch** | TODO<br>类型: 字符串|
| **categories** | category 的容器<br>类型: 数组|
| **category** | 操作种类, 比如 put_obj 代表put对象<br>类型: 字符串|
| **bytes_sent** | 从系统发出去的字节数<br>类型: 整形|
| **bytes_received** | 系统收到的字节数<br>类型: 整形|
| **ops** | 操作次数<br>类型: 整形|
| **successful_ops** | 成功的操作次数<br>类型: 整形|
| **summary** | 总的统计信息, 以用户为单位<br>类型: 数组|
| **total** | 单个用户总的操作信息<br>类型: json 对象|

#### 错误响应消息
请参考[错误响应描述](../error.md)
### 示例:
#### 请求实例
```
GET /admin/usage?format=json&uid=lucy&show-entries=False&start=2015-11-18&end=2015-11-19 HTTP/1.1
Host: obs.eayun.com
Accept-Encoding: gzip, deflate
Accept: */*
User-Agent: python-requests/2.4.3 CPython/2.7.9 Linux/3.19.0-21-generic
Connection: keep-alive
date: Fri, 20 Nov 2015 04:18:12 GMT
Authorization: AWS 8XHJDGE7265UBJP43DIT:eUnZwgKlErCBDodpIqNWyXUZzos=
```
#### 响应实例
```
HTTP/1.1 200 
Date: Fri, 20 Nov 2015 04:18:12 GMT
Server: Apache/2.4.6 (CentOS) OpenSSL/1.0.1e-fips
Connection: close
Transfer-Encoding: chunked
Content-Type: application/json
Set-Cookie: RADOSGWLB=ceph2; path=/
Cache-control: private

{
    "summary": [
        {
            "categories": [
                {
                    "bytes_received": 0,
                    "bytes_sent": 0,
                    "category": "create_bucket",
                    "ops": 1,
                    "successful_ops": 1
                },
                {
                    "bytes_received": 0,
                    "bytes_sent": 900,
                    "category": "list_buckets",
                    "ops": 3,
                    "successful_ops": 3
                },
                {
                    "bytes_received": 27375976,
                    "bytes_sent": 0,
                    "category": "put_obj",
                    "ops": 4,
                    "successful_ops": 4
                }
            ],
            "total": {
                "bytes_received": 27375976,
                "bytes_sent": 900,
                "ops": 8,
                "successful_ops": 8
            },
            "user": "lucy"
        }
    ]
}
```

> ###### 注意
> 为了方便显示, 返回的 json 字符串做了排版
