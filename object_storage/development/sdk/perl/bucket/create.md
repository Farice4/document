### 创建桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `add_bucket` 方法创建桶。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 创建桶
my $bucket = $conn->add_bucket({bucket => 'my-new-bucket'});
```
