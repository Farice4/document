### 环境准备
1. Ruby 开发环境。

2. 安装 `awk-sdk` Gem 包，版本为 ‘~> 2'。

### 初始化

要使用易云云存储，需要使用账户自己的 `access_key` 和 `secret_key` 来进行认证并初始化一个 Ruby SDK 中的 `Client` 实例，在这一 `Client` 实例的基础上完成其它操作。

```
require 'aws-sdk'

Aws.config.update(
    endpoint: 'http://obs.eayun.com',
    region: 'bj',
    access_key_id: '9FSTXQR54VKR7VEM7GAJ',
    secret_access_key: 'TRYrqiFTwIC8h6hw1jFQKd5o4RHfzIsjje4qawOG',
    signature_version: 's3'
)

eayunobs_client = Aws::S3::Client.new
```
