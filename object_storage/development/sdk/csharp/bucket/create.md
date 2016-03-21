### 创建 bucket

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 调用 **PutBucketRequest()** 创建 bucket

#### 示例

创建一个名为 "my-new-bucket" 的 bucket

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    PutBucketRequest request = new PutBucketRequest();
    request.BucketName = "my-new-bucket";
    client.PutBucket(request);
}
```
