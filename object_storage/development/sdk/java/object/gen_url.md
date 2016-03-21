### 生成对象URL
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`generatePresignedUrl`方法。

#### 示例

获取桶my-bucket中的hello.txt的下载链接。

```
import com.amazonaws.services.s3.model.GeneratePresignedUrlRequest;

//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";

//构造一个链接生成的请求
GeneratePresignedUrlRequest request = new GeneratePresignedUrlRequest(bucket, obj);

//生成链接
URL url = client.generatePresignedUrl(request);
System.out.println(url);

```

输出：

```
http://my-bucket.obs.eayun.com:9090/hello.txt?AWSAccessKeyId=G7HGAZI01NOYBNWQ4EJD&Expires=1449467890&Signature=JrY849gEpLQqbJn5e6M3Tvy03to%3D
```
