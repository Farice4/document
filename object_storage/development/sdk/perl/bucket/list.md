### 获取用户的所有bucket

#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用实例的 `buckets` 方法，获取桶列表。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 获取桶列表
my $buckets = @{$conn->buckets->{buckets} || []};
foreach my $bucket (@buckets) {
    print $bucket->bucket . "\t" . $bucket->creation_date . "\n";
}
```
