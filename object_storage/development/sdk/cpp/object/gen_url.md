### 生成对象匿名URL

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 设置相关回调函数, 调用 **S3_delete_object()** 删除对象
* 完成请求调用 **S3_deinitialize*()* 释放资源

#### 示例

下例为 "sample_bucket/world.txt" 生成一个匿名的公共URL, 有效时间为五分钟(五分钟之后不可匿名访问)

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
#include <time.h>

S3BucketContext bucketContext =
{
        host,
        "sample_bucket",
        S3ProtocolHTTP,
        S3UriStylePath,
        access_key,
        secret_key
};

int main(int argc, char *argv[])
{
    const char sample_key[] = "world.txt";
    S3_initialize("s3", S3_INIT_ALL, host);

    char buffer[S3_MAX_AUTHENTICATED_QUERY_STRING_SIZE];
    int64_t expires = time(NULL) + 60 * 5; // 当前时间加五分钟

    S3_generate_authenticated_query_string(
        buffer, &bucketContext, sample_key, expires, NULL);
    std::cout << buffer << std::endl;
    S3_deinitialize();
    
    return 0;
}
```
