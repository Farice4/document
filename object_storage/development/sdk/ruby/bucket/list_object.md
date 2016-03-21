### 查看桶内对象的列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `list_objects` 方法获取对象列表。

#### 示例
```
# 初始化 Client 实例
client = ...

# 查看桶内对象列表
bucket_name = 'new-bucket'
client.list_objects(bucket: bucket_name)
```
