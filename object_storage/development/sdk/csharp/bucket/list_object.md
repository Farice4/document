### 获取 bucket 内的对象列表

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 调用 **DeleteBucketRequest()** 删除 bucket

#### 示例

列出 "my-new-bucket" 的对象

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    ListObjectsRequest request = new ListObjectsRequest();
    request.BucketName = "my-new-bucket";
    ListObjectsResponse response = client.ListObjects(request);
    foreach (S3Object o in response.S3Objects)
    {
        Console.WriteLine("{0}\t{1}\t{2}", o.Key, o.Size, o.LastModified);
    }
}
```
