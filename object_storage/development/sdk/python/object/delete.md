### 删除对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
有两种方法：
* 在目标桶实例上调 `delete_key` 方法：
    1. 初始化 `S3Connection` 实例；
    2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
    3. 在桶实例上调用 `delete_key` 删除对象。
* 在目标对象实例上调 `delete` 方法：
    1. 初始化 `S3Connection` 实例；
    2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
    3. 在桶实例上调用 `get_key` 获取目标对象的实例（ `boto.s3.key` ）；
    4. 在对象的实例上调用以 `delete` 方法删除对象。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 在桶实例上调用 delete 方法删除对象
object_name = 'new-object'
bucket.delete_key(object_name)

# 或者获取对象实例后调用 delete 方法
# new_object = bucket.get_key(object_name)
# new_object.delete()
```
