### 删除对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `delete_object` 方法删除对象。

#### 示例
```
# 初始化 Client 实例
client = ...

# 上传对象
bucket_name = 'new-bucket'
object_name = 'new-object'
client.delete_object(bucket: bucket_name, key: object_name)
```
