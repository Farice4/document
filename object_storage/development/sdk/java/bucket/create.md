### 创建桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`createBucket`方法。

#### 示例

创建一个名叫my-bucket的桶。

```
//初始化
AmazonS3 client = ...

//创建桶
String key = "my-bucket"
String buc = client.createBucket(key);
system.out.printf(buc);
```

输出
```
my-bucket
```
