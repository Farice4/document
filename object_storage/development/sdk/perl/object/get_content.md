### 获取对象内容
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标对象的读取权限。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `bucket` 方法获取桶；
3. 调用 `get_key_filename` 方法获取对象内容。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 获取桶对象
my $bucket = $conn->bucket('my-new-bucket');

# 获取对象内容
my $key = 'hello.txt';
my $local_path = '/path/to/file'
$bucket->get_key_filename($key, undef, $local_path);
```
