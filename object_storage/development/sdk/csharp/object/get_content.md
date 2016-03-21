### 获取对象内容

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 初始化 S3 连接
* 创建一个 **GetObjectRequest** 的对象, 多相关的设置.
* 调用 **GetObjectResponse::WriteResponseStreamToFile()** 保存文件

#### 示例

将 "my-new-bucket" 中的 docker_practice.pdf 下载到用户文档下. 

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
void DoRequest()
{
            GetObjectRequest request = new GetObjectRequest();
            request.BucketName = "my-new-bucket";
            request.Key        = "docker_practice.pdf";
            GetObjectResponse response = client.GetObject(request);
            response.WriteResponseStreamToFile("C:\\Users\\dunrong\\Documents\\docker_practice.pdf");
}
```
