### 完成针对对象发起多段上传任务
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 初始化 `Client` 实例；
2. 调用 `complete_multipart_upload` 方法完成多段上传任务。

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

# 上传各部份数据
mpup_id = mpup_task.upload_id
...

# 分别上传各部份
part_1 = client.upload_part(...)
...

mpup_parts = {
  parts: [
    {
      etag: part_1.etag,
      part_number: 1,
    },
    ...
  ],
}

# 完成多段上传任务
client.complete_multipart_upload(
    bucket: bucket_name,
    key: object_name,
    multipart_upload: mpup_parts
    upload_id: mpup_id
)
```
