### 在多段上传任务上传一段数据
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 发起多段上传任务之后，获取多段上传任务的实例（ `boto.s3.multipart` ）；
2. 在多段上传任务实例上调用 `upload_part_from_file` 方法上传内容。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 发起多段上传任务
object_name = 'new-object'
mpup_task = bucket.initiate_multipart_upload(object_name)

# 上传本段内容
part_number = 1
with open('/source/file/path', 'r') as file:
    mpup_task.upload_part_from_file(file, part_number)
```
