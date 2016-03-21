### 在多段上传任务上传一段数据
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `upload_part` 方法上传内容。

#### 示例
```
# 初始化 Client 实例
client = ...

# 发起多段上传任务
bucket_name = 'new-bucket'
object_name = 'new-object'
mpup_task = client.create_multipart_upload(
    bucket: bucket_name,
    key: object_name
)

# 上传本段内容
mpup_id = mpup_task.upload_id
File.open('/source/file/path', 'rb') do |file|
    client.upload_part(
        body: file,
        bucket: bucket_name,
        key: object_name,
        part_number: 1,
        upload_id: mpup_id
    )
end
```
