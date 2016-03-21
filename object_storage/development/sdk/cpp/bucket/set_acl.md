### 设置桶的访问控制权限

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 设置相关的权限变量, 调用 **S3_set_acl()** 设置 ACL
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

下例将 "sample_bucket" 的所有权限赋予 testobs 用户, 将权限的读赋予 testuser2 用户, 将读权限赋予所有用户.

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
#include <string.h>

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
    char ownerId[] = "testobs";
    char ownerDisplayName[] = "testobs";
    char granteeId[] = "testuser2";
    char granteeDisplayName[] = "testuser2";

    S3AclGrant grants[] = {
        {
        // 所有控制权限
            S3GranteeTypeCanonicalUser,
            {{}},
            S3PermissionFullControl
        },
        {
        // 读取权限的权限
            S3GranteeTypeCanonicalUser,
            {{}},
            S3PermissionReadACP
        },
        {
        // 读权限
            S3GranteeTypeAllUsers,
            {{}},
            S3PermissionRead
        }
    };

    // 将 "sample_bucket" 的所有权限赋予 testobs 用户.
    strncpy(grants[0].grantee.canonicalUser.id, ownerId, S3_MAX_GRANTEE_USER_ID_SIZE);
    strncpy(grants[0].grantee.canonicalUser.displayName, ownerDisplayName, 
            S3_MAX_GRANTEE_DISPLAY_NAME_SIZE);

    // 将 "sample_bucket" 的读赋予 testuser2 用户
    strncpy(grants[1].grantee.canonicalUser.id, granteeId, S3_MAX_GRANTEE_USER_ID_SIZE);
    strncpy(grants[1].grantee.canonicalUser.displayName, granteeDisplayName, 
            S3_MAX_GRANTEE_DISPLAY_NAME_SIZE);

    S3_set_acl(&bucketContext, NULL, ownerId, ownerDisplayName, 3, grants, 0, 
               &responseHandler, 0);
    S3_deinitialize();
    
    return 0;
}
```
