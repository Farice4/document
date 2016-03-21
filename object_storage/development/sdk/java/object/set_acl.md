### 设置对象的ACL
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`setObjectAcl`方法。

#### 示例

设置对象hello.txt为PublicRead。

```
import com.amazonaws.services.s3.model.CannedAccessControlList;

//初始化
AmazonS3 client = ...

String obj = "hello.txt";
String buc = "my-bucket";

//设置ACL
client.setObjectAcl(buc, obj, CannedAccessControlList.PublicRead)
```
