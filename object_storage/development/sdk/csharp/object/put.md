### 上传对象

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 调用 **PutObject()** 设置 bucket 的访问权限

#### 示例

将一个文本文件 hello.txt 传到名为 "my-new-bucket" 的 bucket

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    PutObjectRequest request = new PutObjectRequest();
    request.BucketName      = "my-new-bucket";
    request.Key         = "hello.txt";
    request.ContentType = "text/plain";
    request.ContentBody = "Hello World!";
    client.PutObject(request);
}
```
