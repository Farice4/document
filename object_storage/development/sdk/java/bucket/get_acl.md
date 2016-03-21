### 获取桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`getBucketAcl`方法。

#### 示例
```
//初始化
AmazonS3 client = ...

String buc = "my-bucket";

//构造一个获取ACL请求
GetBucketAclRequest bar = new GetBucketAclRequest(buc);

//获取ACL
AccessControlList acl = client.getBucketAcl(bar);
```
