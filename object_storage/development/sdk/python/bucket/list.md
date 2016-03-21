### 获取桶列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `S3Connection` 实例；
2. 调用 `get_all_buckets` 方法获取桶列表。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶列表
for bucket in conn.get_all_buckets():
    print "{name}\t{created}".format(
        name = bucket.name,
        created = bucket.creation_date,
    )
```
