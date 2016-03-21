### 获取对象访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标对象访问控制信息的读取权限。

#### 流程说明
1. 初始化 `S3Connection` 实例；
2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
3. 在桶实例上调用 `get_key` 获取对象实例（ `boto.s3.key` ）；
4. 在对象的实例上调用以 `get_acl` 获取对象访问控制权限。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 获取对象实例
object_name = 'new-object'
new_object = bucket.get_key(object_name)

# 获取对象访问控制权限
new_object_acl_policy = new_object.get_acl()
```
