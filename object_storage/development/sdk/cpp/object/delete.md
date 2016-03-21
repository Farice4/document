### 删除一个对象

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 设置相关回调函数, 调用 **S3_delete_object()** 删除对象
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

下例将 "sample_bucket/world.txt" 删除

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
#if 1
S3BucketContext bucketContext =
{
        host,
        "sample_bucket",
        S3ProtocolHTTP,
        S3UriStylePath,
        access_key,
        secret_key
};
#endif

int main(int argc, char *argv[])
{
    const char sample_key[] = "world.txt";
    S3_initialize("s3", S3_INIT_ALL, host);

    S3ResponseHandler deleteResponseHandler = {
        0,
        &responseCompleteCallback
    };
    S3_delete_object(&bucketContext, sample_key, 0, &deleteResponseHandler, 0);    
    S3_deinitialize();
    
    return 0;
}
```
