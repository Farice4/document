### 获取对象内容
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`putObject`方法。

#### 示例

下载桶my-bucket中的hello.txt到本地。

```
import com.amazonaws.services.s3.model.GetObjectRequest;

//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";
String localobj = "/home/obs/hello.txt";
File f = new File(localobj);

//构造一个获取对象请求
GetObjectRequest objr = new GetObjectRequest(bucket, obj);

//输出对象ID信息
System.out.println(objr.getS3ObjectId());

//下载
conn.getObject(objr, f);
```
