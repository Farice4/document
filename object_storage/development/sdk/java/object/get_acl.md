### 获取对象访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`getObjectAcl`方法。

#### 示例
```
//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";

//获取对象的ACL
conn.getObjectAcl(buc, obj);
```
