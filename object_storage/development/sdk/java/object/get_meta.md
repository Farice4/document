### 获取对象元数据
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`getObjectMetadata`方法。

#### 示例

下载桶my-bucket中的hello.txt到本地。

```
import com.amazonaws.services.s3.model.GetObjectRequest;

//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";

//构造一个获取对象元数据的请求
GetObjectMetadataRequest gomr = new GetObjectMetadataRequest(buc, obj);

//获取对象元数据
client.getObjectMetadata(gomr);
```
