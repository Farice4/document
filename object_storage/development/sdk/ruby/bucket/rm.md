### 删除桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。
> ###### 注意：
> 不能直接删除非空的桶。对于包含对象的桶，在删除之后，应首先删除其内所有的对象。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `delete_bucket` 方法删除桶。

#### 示例
```
# 初始化 Client 实例
client = ...

# 删除桶
bucket_name = 'new-bucket'
client.delete_bucket(bucket: bucket_name)
```
