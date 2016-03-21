## 查看桶内对象的列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`listObjects`方法。

#### 示例
```
//初始化
AmazonS3 client = ...

String buc = "my-bucket";

//构造一个获取桶对象列表的请求
ListObjectsRequest loq = new ListObjectsRequest();
loq.setBucketName(buc);

//获取对象列表
ObjectListing objects = client.listObjects(loq);
```
