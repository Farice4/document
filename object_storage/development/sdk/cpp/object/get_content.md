### 获取对象内容

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 创建相关的回调函数, 调用 **S3_get_object** 获取对象内容
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

将 "sample_bucket" 中名为 world.txt 的对象下载到本地, 并打印

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
S3BucketContext bucketContext =
{
        host,
        "sample_bucket",
        S3ProtocolHTTP,
        S3UriStylePath,
        access_key,
        secret_key
};

static S3Status getObjectDataCallback(int bufferSize, const char *buffer, void *callbackData)
{
    FILE *outfile = (FILE *) callbackData;
    size_t wrote = fwrite(buffer, 1, bufferSize, outfile);
    return ((wrote < (size_t) bufferSize) ? S3StatusAbortedByCallback : S3StatusOK);
}

int main(int argc, char *argv[])
{
    S3_initialize("s3", S3_INIT_ALL, host);

    S3GetObjectHandler getObjectHandler = {
        responseHandler,
        &getObjectDataCallback
    };
    FILE *outfile = stdout;
    const char sample_key[] = "world.txt";
    S3_get_object(&bucketContext, sample_key, NULL, 0, 0, NULL, &getObjectHandler, outfile);
    S3_deinitialize();
    
    return 0;
}
```
