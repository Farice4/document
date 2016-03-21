### 删除对象

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 初始化 **DeleteObjectRequest()** 对象
* 调用 **DeleteObject()** 设置 bucket 的访问权限

#### 示例

将名为 "my-new-bucket" 中的 hello.txt 设置为为公共可读的, 将 world.txt 设为私有的

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    DeleteObjectRequest request = new DeleteObjectRequest();
    request.BucketName = "my-new-bucket";
    request.Key        = "hello.txt";
    client.DeleteObject(request);
}
```
