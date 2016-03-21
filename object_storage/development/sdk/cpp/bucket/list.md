### 获取用户的所有bucket

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 定义相关的回调函数, 调用 **S3_list_service()** 获取结果
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
static S3Status listServiceCallback(
    const char *ownerId,
    const char *ownerDisplayName,
    const char *bucketName,
    int64_t creationDate, void *callbackData)
{
    bool *header_printed = (bool*) callbackData;
    if (!*header_printed) {
        *header_printed = true;
        printf("%-22s", "       Bucket");
        printf("  %-20s  %-12s", "     Owner ID", "Display Name");
        printf("\n");
        printf("----------------------");
        printf("  --------------------" "  ------------");
        printf("\n");
    }

    printf("%-22s", bucketName);
    printf("  %-20s  %-12s", ownerId ? ownerId : "", ownerDisplayName ? ownerDisplayName : "");
    printf("\n");

    return S3StatusOK;
}

S3ListServiceHandler listServiceHandler =
{
    responseHandler,
    &listServiceCallback
};

int main(int argc, char *argv[])
{
    bool header_printed = false;
    S3_initialize("s3", S3_INIT_ALL, host);
    S3_list_service(S3ProtocolHTTP, access_key, secret_key, host, 0, &listServiceHandler, &header_printed);
    S3_deinitialize();
    return 0;
}
```
