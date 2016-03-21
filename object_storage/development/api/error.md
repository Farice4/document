如果请求遇到错误, 在响应中会包含相应的错误码信息. 入下表所示:

| 错误码 | 描述 | HTTP 状态码 |
| :-- | :-- | :-- |
| **AccessDenied** | 拒绝访问 | 403 |
| **BadDigest** | 客户端指定的对象内容的MD5值与系统接收到的内容MD5值不一致 | 400 |
| **BucketAlreadyExists** | 请求的桶名已经存在 | 409 |
| **BucketNotEmpty** | 用户尝试删除的桶不为空 | 409 |
| **EntityTooSmall** | 用户试图上传的对象大小小于系统允许的最小大小 | 409 |
| **EntityTooLarge** | 用户试图上传的对象大小超过了系统允许的最大大小 | 409 |
| **InvalidAccessKey**** | 系统记录中不存在客户提供的AWS Access Key | 403 |
| **InvalidArgument** | 无效的参数 | 400 |
| **InvalidBucketName** | 请求中指定的桶名无效 | 400 |
| **InvalidObjectName** | 请求中指定的对象名无效 | 400 |
| **InvalidDigest** | HTTP头中指定的Content-MD5值无效 | 400 |
| **InvalidPart** | 一个或多个指定的段无法找到。这些段可能没有上传，或者指定的entity tag与段的entity tag不一致. | 400 |
| **InvalidPartOrder** | 段列表的顺序不是升序，段列表必须按段号升序排列. | 400 |
| **InvalidRange** | 请求的range不可获得 | 416 |
| **InvalidRequest** | 无效请求 | 400 |
| **Locked** | 更新元数据或者元数据日志时加锁失败，建议重新尝试。| 423 |
| **MethodNotAllowed** | 指定的方法不允许操作在请求的资源上 | 405 |
| **MissingContentLength** |必须要提供HTTP消息头中的Content-Length字段 | 411 |
| **NoSuchBucket** |指定的桶不存在 | 404 |
| **NoSuchKey** | 指定的对象不存在 | 404 |
| **NoSuchUpload** | 指定的多段上传不存在。Upload ID不存在，或者多段上传已经终止或完成。 | 404 |
| **RequestTimeout** | 用户与Server之间的socket连接在超时时间内没有进行读写操作。 | 400 |
| **NotModified** | 指定对象的内容未被修改 | 304 |
| **TooManyBuckets** | 用户拥有的桶的数量达到了系统的上限，并且请求试图创建一个新桶 | 400 |
| **UnresolvableGrantByEmailAddress** | 用户提供的Email与记录中任何帐户的都不匹配 | 400 |
| **PermanentRedirect** | 尝试访问的桶必须使用指定的结点，请将以后的请求发送到这个结点 | 301 |
| **UserSuspended** | 用户被冻结使用 | 403 |
| **RequestTimeTooSkewed** | 请求的时间与服务器的时间相差太大。 | 403 |
| **Not Found** | 资源未找到 | 404 |
| **PreconditionFailed** | 用户指定的先决条件中至少有一项没有包含 | 412 |
| **QuotaExceeded**| 创建桶或对象时数量已超过了最大配额 | 403 |
| **RequestTimeout** | 用户与Server之间的socket连接在超时时间内没有进行读写操作 | 400 |
| **UnprocessableEntity** | 创建对象时 Etag 校验失败 | 422 |
| **InternalError** | 内部错误 | 500 |
