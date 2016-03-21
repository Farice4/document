### 删除对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`deleteObject`方法。

#### 示例

删除桶my-bucket中的hello.txt。

```
//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";

//删除
client.deleteObject(buc, obj);
```
