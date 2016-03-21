### 环境准备
1. Python 开发环境。

2. 安装 `boto` ，使用 `pip` 或者系统自带包管理器。

### 初始化

要使用易云云存储，需要使用账户自己的 `access_key` 和 `secret_key` 来进行认证并初始化一个 Python SDK 中的 `S3Connection` （ `boto.s3.connection` ） 实例，在这一 `S3Connection` 实例的基础上完成其它操作。

```
import boto
import boto.s3.connection

access_key = 'BDNGQWVO6BCMTF5YTKKF'
secret_key = 'VMj2y+EKBEfTfDuoXDi1ddLW6rqqu2HOsE+dDUuf'

eayunobs_conn = boto.connect_s3(
    aws_access_key_id = access_key,
    aws_secret_access_key = secret_key,
    host = 'obs.eayun.com',
    )
```
