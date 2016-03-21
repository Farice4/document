### 上传对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 并且该用户拥有目标桶的写入权限。

#### 流程说明
1. 初始化 `S3Connection` 实例；
2. 通过 `get_bucket` 获取目标桶的实例（ `boto.s3.bucket` ）；
3. 在桶实例上调用 `new_key` 创建对象，返回新建对象的实例（ `boto.s3.key` ）；
4. 在对象的实例上调用以 `set_contents_from_` （ 可用的方法包括 `file` 、 `filename` 、`stream` 、 `string` ）等方法上传数据。

#### 示例
```
# 初始化 S3Connection 实例
conn = ...

# 获取桶实例
bucket_name = 'new-bucket'
bucket = conn.get_bucket(bucket_name)

# 创建新的对象
object_name = 'new-object'
new_object = bucket.new_key(object_name)

# 上传对象
with open('/source/file/path', 'r') as file:
    new_object.set_contents_from_file(file)
# 或者使用 set_contents_from_filename
# new_object.set_contents_from_filename('/source/file/path')
# 或者使用 set_contents_from_string
# new_object.set_contents_from_string('Hello World!')
```
