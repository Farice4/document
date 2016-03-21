#### 查看桶内对象的列表
##### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

##### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `list_bucket_all` 方法获取对象列表。

##### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 查看桶内对象列表
my @keys = @{$conn->list_bucket_all({bucket=>'my-new-bucket'})->{keys} || []};
foreach my $key (@keys) {
    print "$key->{key}\t$key->{size}\t$key->{last_modified}\n";
}
```
