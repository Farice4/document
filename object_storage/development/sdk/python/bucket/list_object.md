### 查看桶内对象的列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `S3Connection` 实例；
2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
3. 在桶实例上调用 `list` 方法获取对象列表。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 查看桶内对象列表
for key in bucket.list():
    print "{name}\t{size}\t{modified}".format(
        name = key.name,
        size = key.size,
        modified = key.last_modified
    )
```
