### 查询桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `bucket` 方法获取桶对象；
3. 调用桶对象的 `get_acl` 方法查询桶的访问控制权限，返回值是一个 XML 文档。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 获取桶对象
my $bucket = $conn->bucket('my-new-bucket');

# 查询桶的访问控制权限
$bucket->get_acl();
```
