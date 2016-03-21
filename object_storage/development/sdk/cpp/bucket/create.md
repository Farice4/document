### 创建 bucket

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 定义相关的回调函数, 调用 **S3_create_bucket()** 创建 bucket
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

创建名为 "sample_bucket" 的 bucket

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
// 对于 bucket 的相关操作, 入查看, 删除, 创建等, 需要一个 S3BucketContext 的对象.
int main(int argc, char *argv[])
{
    const char sample_bucket[] = "sample_bucket";
    S3_initialize("s3", S3_INIT_ALL, host);
    S3_create_bucket(S3ProtocolHTTP, access_key, secret_key, host, 
                     sample_bucket, S3CannedAclPrivate, NULL, NULL, 
                     &responseHandler, NULL);
    S3_deinitialize();
    return 0;
}
```
