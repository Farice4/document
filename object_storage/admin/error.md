如果请求遇到哦错误, 在响应中会包含相应的错误码信息. 入下表所示:

| 错误码 | 描述 | HTTP 状态码 |
| :-- | :-- | :-- |
| **UserExists** | 需要创建的用户已经存在 | 409 |
| **InvalidAccessKey** | 提供的key无效 | 400 |
| **InvalidKeyType** | 提供的key的类型无效 | 400 |
| **InvalidSecretKey** | 提供的secret的错误 | 400 |
| **KeyExists** | 提供的key已经存在, 并且属于另一个用户 | 409 |
| **EmailExists** | 提供的邮件地址已经存在 | 409 |
| **InvalidCability** | 提供的 capability 无效 | 409 |
| **SubuserExists** | 需要创建的subuser已经存在 | 409 |
