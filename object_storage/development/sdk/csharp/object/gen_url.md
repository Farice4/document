### 生成对象匿名URL

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 初始化 **GetPreSignedUrlRequest()** 对象
* 调用 **GetPreSignedURL()** 得到匿名的 URL.

#### 示例

下例为 "/my-new-bucket/hello.txt" 生成一个匿名的公共URL, 有效时间为一个小时(一个小时之后不可匿名访问)

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
    GetPreSignedUrlRequest request = new GetPreSignedUrlRequest();
    request.BucketName = "my-new-bucket";
    request.Key        = "hello.txt";
    request.Expires    = DateTime.Now.AddHours(1);
    request.Protocol   = Protocol.HTTP;
    string url = client.GetPreSignedURL(request);
    Console.WriteLine(url);
}
```
