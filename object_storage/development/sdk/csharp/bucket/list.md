### 获取用户的所有bucket

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 调用 **s3Client.ListBuckets()** 获取结果

#### 示例

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    ListBucketsResponse response = client.ListBuckets();
    foreach (S3Bucket b in response.Buckets)
    {
        Console.WriteLine("{0}\t{1}", b.BucketName, b.CreationDate);
    }
}
```
