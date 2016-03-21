### 完成针对对象发起多段上传任务
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 发起多段上传任务之后，获取多段上传任务的实例（ `boto.s3.multipart` ）；
2. 在多段上传任务实例上调用 `upload_part_from_file` 方法上传内容。
3. 所有数据上传完毕之后调用 `complete_upload` 方法完成多段上传任务。

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

# 分别上传各部份数据
...
mpup_task.upload_part_from_file(file, part_number)
...

# 完成多段上传任务
mpup_task.complete_upload()
```
