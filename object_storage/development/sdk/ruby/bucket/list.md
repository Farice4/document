### 获取桶列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `list_buckets` 方法获取桶列表。

#### 示例
```
# 初始化 Client 实例
client = ...

# 获取桶列表
client.list_buckets.buckets.each do |bucket|
    puts "#{bucket.name}\t#{bucket.creation_date}"
end
```
