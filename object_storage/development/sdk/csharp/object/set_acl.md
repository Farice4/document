### 设置对象的访问控制权限

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 调用 **PutACL()** 设置 bucket 的访问权限

#### 示例

将名为 "my-new-bucket" 中的 hello.txt 设置为为公共可读的, 将 world.txt 设为私有的

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    PutACLRequest request = new PutACLRequest();
    request.BucketName = "my-new-bucket";
    request.Key        = "hello.txt";
    request.CannedACL  = S3CannedACL.PublicRead;
    client.PutACL(request);

    PutACLRequest request = new PutACLRequest();
    request.BucketName = "my-new-bucket";
    request.Key        = "world.txt";
    request.CannedACL  = S3CannedACL.Private;
    client.PutACL(request);
}
```
