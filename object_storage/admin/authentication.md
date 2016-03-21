# 授权认证
EayunOBS 像用户提供了包括 AK(Access Key ID) 和 SK(Secret Access Key) 的帐号信息, 用于对用户的请求进行认证. 目前 EayunOBS 系统支持两个版本的认证方式，V2和V4认证. V4 认证是相对更安全的认证方式.
## V2 认证
在请求头中, V2 认证的认证信息格式如下:
```
Authorization: AWS AccessKeyId:Signature
```
AccessKeyId 是上面提到的 AK, Signature 是请求中选定元素的 RFC 2104HMAC-SHA1, 授权标头的 Signature 部分会因请求的不同而各异。如果系统计算出的请求签名与请求随附的 Signature 相匹配，则请求者拥有 EayunOBS 的私有访问密钥。然后才可以使用此密钥请求服务.

构造 Signature 分为两部分:
### 构造 StringToSign
StringToSign 的格式如下:
```python
StringToSign = HTTP-Verb + "\n" +
    Content-MD5 + "\n" +
    Content-Type + "\n" +
    Date + "\n" +
    CanonicalizedEayunOBSHeaders +
    CanonicalizedResource
```
对上面的参数说明如下:

| 消息头名字 | 描述 |
| :-- | :-- |
|**HTTP-Verb**|接口操作的方法，如："GET"，"PUT"，"DELETE"等|
|**Content-MD5**|经过Base64编码的请求消息体的MD5值, 如果不提供此项, 可以留空字符串|
|**Content-Type**|消息内容的 MIME 类型, 比如: text/plain|
|**Date**|生成请求的时间, 该时间格式遵循RFC 1123, 当有自定义字段x-amz-date时，该参数按照空字符串处理|
|**CanonicalizedEayunOBSHeaders**|EayunOBS自定义的字段，以“x-amz-”作为前辍的消息头，如“x-amz-date，x-amz-acl”。构建步骤:<br>1. 将每个 HTTP 标头名称转换为小写。例如，“X-Amz-Date”改为“x-amz-date”。<br>2. 根据标头名称按字典顺序排列标头集。<br>3. 合并有相同名称的标头字段, 例如，可以将元数据标头“x-amz-meta-id:id1”和“x-amz-meta-id:id2”合并为单个标头“x-amz-meta-id:id1,id2”。<br>4. 自定义字段中含有无意义空格或table键时，需要摒弃。例如："x-amz-meta-id: id1"，需要转换为：x-amz-meta-id:id1<br>5. <br>6. 每一个自定义字段最后都需要另起新行.
|**CanonicalizedResource**|CanonicalizedResource 表示请求的目标 EayunOBS 资源. 格式为:<br>CanonicalizedResource = [ "/" + BucketName ] + [HTTP-Request-URI] +	[subresource];<br>构建步骤:<br>1. 如果以虚拟主机方式访问，需要将桶名添加进来，如果不是虚拟主机方式，则不做处理，例如: <br>对于虚拟托管类型请求“https://m8x.obs.eayun.com/images/foo.jpg”, CanonicalizedResource 是“/m8x”。<br>对于路径类型请求“https://obs.eayun.com/m8x/images/foo.jpg”, CanonicalizedResource 是 ""。<br>2. 附加未解码的 HTTP 请求 HTTP-Request-URI部分, 例如:<br>对于虚拟托管类型请求“https://m8x.obs.eayun.com/images/foo.jpg”, CanonicalizedResource 是“/m8x/images/foo.jpg”<br>对于路径类型请求“https://obs.eayun.com/m8x/images/foo.jpg”, CanonicalizedResource 是“/m8x/images/foo.jpg”<br>3. 如果有子资源, 则将子资源添加进来，例如?acl，?logging|

生成StringToSign的例子:
//TODO
### 生成 Signature
使用 HMAC(hash-based authentication code) 根据 StringToSign和SecretAccessKeyID生成Signature，伪代码如下
```
Signature = Base64(HMAC-SHA1(SecretAccessKeyID, UTF-8-Encoding-Of(StringToSign)));
```

## V4 认证
//TODO
