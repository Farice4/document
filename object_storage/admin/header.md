# 公共消息头
## 公共请求消息头

TODO: Authentication 链接

| 消息头名字 | 描述 | 必选 |
| :-- | :-- | :-- |
| **Authorization** | 授权信息, 信息的内容生成参考下一节 | 匿名请求不需要, 其他请求必选 |
|**Content-Length**|RFC 2616中定义的消息长度|PUT操作和加载XML的操作必选|
|**Content-Type**|资源内容的类型，例如: text/plain|否|
|**Date**|请求发起的时间, 比如:Mon, 16 Nov 2015 03:54:11 +0000|匿名请求不用, 其他请求 x-amz-date 和 Date 字段必须带一个|
|**Host**|主机地址,对于路径类型的请求，该值为服务端的域名，如obs.eayun.com, 对于虚拟主机方式的请求, 该值为桶名加域名, 如 bucketname.obs.eayun.com|是|
|**x-amz-date**|请求发起的时间, 比如:Mon, 16 Nov 2015 03:54:11 +0000|匿名请求不用, 其他请求 x-amz-date 和 Date 字段必须带一个, 如果两者都有,只有x-amz-date有效|

## 公共响应消息头
| 消息头名字 | 描述 |
| :-- | :-- |
|**Content-Length**|消息体的长度, 比如: Content-Length: 5850|
|**Content-Type**|消息内容的 MIME 类型, 比如: Content-Type: text/plain; charset=UTF-8|
|**Connection**|服务器的连接是打开的还是关闭的|
|**Date**|EayunOBS系统相应时的时间|
|**Etag**|对象内容的 Hash 值, 与对象的元数据无关|
|**Server**|相应服务器的名字|
