### 复制对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有源对象的读取权限、目标对象所在桶的写入权限。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `copy_object` 方法复制对象。

#### 示例
```
# 初始化 Client 实例
client = ...

# 复制对象
bucket_name = 'new-bucket'
source_object = 'old-object'
dest_object = 'new-object'
client.copy_object(
    bucket: bucket_name,
    copy_source: '/' + bucket_name + '/' + object_name,
    key: dest_object)
```
