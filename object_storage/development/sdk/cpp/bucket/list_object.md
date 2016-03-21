### 获取 bucket 内的对象列表

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 定义相关的回调函数, 调用 **S3_list_bucket()** 创建 bucket
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

列出名为 "sample_bucket" 的 bucket 里的对象列表

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
static S3Status listBucketCallback(
    int isTruncated,
    const char *nextMarker,
    int contentsCount,
    const S3ListBucketContent *contents,
    int commonPrefixesCount,
    const char **commonPrefixes,
    void *callbackData)
{
    printf("%-22s", "      Object Name");
    printf("  %-5s  %-20s", "Size", "   Last Modified");
    printf("\n");
    printf("----------------------");
    printf("  -----" "  --------------------");
    printf("\n");

    for (int i = 0; i < contentsCount; i++) {
        char timebuf[256];
        char sizebuf[16];
        const S3ListBucketContent *content = &(contents[i]);
        time_t t = (time_t) content->lastModified;

        strftime(timebuf, sizeof(timebuf), "%Y-%m-%dT%H:%M:%SZ", gmtime(&t));
        sprintf(sizebuf, "%5llu", (unsigned long long) content->size);
        printf("%-22s  %s  %s\n", content->key, sizebuf, timebuf);
    }

    return S3StatusOK;
}

S3ListBucketHandler listBucketHandler =
{
        responseHandler,
        &listBucketCallback
};

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
    S3_initialize("s3", S3_INIT_ALL, host);
    S3_list_bucket(&bucketContext, NULL, NULL, NULL, 0, NULL, 
                   &listBucketHandler, NULL);
    S3_deinitialize();
    return 0;
}
```
