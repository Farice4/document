### 查询桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `get_bucket_acl` 方法查询桶的访问控制权限。

#### 示例
```
# 初始化 Client 实例
client = ...

# 删除桶
bucket_name = 'new-bucket'
client.get_bucket_acl(bucket: bucket_name)
```
