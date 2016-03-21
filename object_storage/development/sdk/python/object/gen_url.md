### 生成对象 URL
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
有两种方法：
* 在 `S3Connection` 实例上调 `generate_url` 方法：
    1. 初始化 `S3Connection` 实例；
    2. 在 `S3Connection` 实例上调用 `generate_url` 生成 URL 。
* 在目标对象实例上调 `generate_url` 方法：
    1. 初始化 `S3Connection` 实例；
    2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
    3. 在桶实例上调用 `get_key` 获取目标对象的实例（ `boto.s3.key` ）；
    4. 在对象的实例上调用以 `generate_url` 方法生成 URL 。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 生成对象 URL
# 有效时间： 60s
conn.generate_url(60, 'GET', bucket='new-bucket', key='new-object')

# 或者在目标对象的实例上调用 generate_url 方法
# 获取桶实例
#bucket_name = 'new-bucket'
#bucket = conn.get_bucket(bucket_name)
#
# 获取对象实例
#object_name = 'new-object'
#new-object = bucket.get_key(object_name)
#
# 生成对象 URL
# 有效时间： 60s
# new_object.generate_url(60)
```
