###  bucket 内的对象列表

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 定义相关的回调函数, 调用 **S3_list_bucket()** 创建 bucket
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

名为 "sample_bucket" 的 bucket 里的对象列表

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
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

#include <string.h>

int main(int argc, char *argv[])
{
    S3_initialize("s3", S3_INIT_ALL, host);
    char ownerId[] = "testobs";
    char ownerDisplayName[] = "testobs";
    char granteeId[] = "testuser2";
    char granteeDisplayName[] = "testuser2";

    S3AclGrant grants[] = {
        {
            S3GranteeTypeCanonicalUser,
            {{}},
            S3PermissionFullControl
        },
        {
            S3GranteeTypeCanonicalUser,
            {{}},
            S3PermissionReadACP
        },
        {
            S3GranteeTypeAllUsers,
            {{}},
            S3PermissionRead
        }
    };

    strncpy(grants[0].grantee.canonicalUser.id, ownerId, S3_MAX_GRANTEE_USER_ID_SIZE);
    strncpy(grants[0].grantee.canonicalUser.displayName, ownerDisplayName, S3_MAX_GRANTEE_DISPLAY_NAME_SIZE);

    strncpy(grants[1].grantee.canonicalUser.id, granteeId, S3_MAX_GRANTEE_USER_ID_SIZE);
    strncpy(grants[1].grantee.canonicalUser.displayName, granteeDisplayName, S3_MAX_GRANTEE_DISPLAY_NAME_SIZE);

    S3_set_acl(&bucketContext, NULL, ownerId, ownerDisplayName, 3, grants, 0, &responseHandler, 0);
    S3_deinitialize();
    
    return 0;
}
