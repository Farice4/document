### 创建桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `create_bucket` 方法创建桶。

#### 示例
```
# 初始化 Client 实例
client = ...

# 创建桶
bucket_name = 'new-bucket'
client.create_bucket(bucket: bucket_name)
```
