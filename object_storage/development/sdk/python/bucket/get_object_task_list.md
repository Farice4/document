### 查询针对桶内对象的多段上传任务列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `S3Connection` 实例；
2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
2. 在桶实例上调用 `list_multipart_uploads` 方法查询针对桶内对象的多段上传任务列表。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 查询多段上传任务列表
for upload in bucket.list_multipart_uploads():
    print "{upload_id}\t{key_name}\t{initiated}\t{initiator}".format(
        upload_id = upload.id,
        key_name = upload.key_name,
        initiated = upload.initiated,
        initiator = upload.initiator.id,
    )
```
