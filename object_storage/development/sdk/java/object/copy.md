### 复制对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`copyObject`方法。

#### 示例
```
//初始化
AmazonS3 client = ...

//源对象
String buc = "my-bucket";
String obj = "hello.txt";

//目的对象
String desBuc = "my-bucket";
String desObj = "hello-copy.txt";

//构造一个复制对象的请求
CopyObjectRequest cor = new CopyObjectRequest(buc, obj, desBuc, desObj);

//复制对象
client.copyObject(cor);
```

