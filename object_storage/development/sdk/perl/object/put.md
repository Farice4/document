### 上传对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `bucket` 方法获取桶；
3. 调用 `add_key_filename` 或者 `add_key` 方法上传对象。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 获取桶对象
my $bucket = $conn->bucket('my-new-bucket');

# 将本地文件上传为对象
my $key = 'new-bucket';
my $value = '/source/file/path';
$bucket->add_key_filename($key, $value, {content_type => 'text/plain'});

# 使用初始内容上传为一个新对象
my $key = 'hello.txt';
my $value = 'Hello world!';
$bucket->add_key($key, $value, {content_type => 'text/plain'});
```
