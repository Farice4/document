## 设置桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`setBucketAcl`方法。

#### 示例
```
//初始化
AmazonS3 client = ...

String buc = "my-bucket";

//构造一个ACL，授予`newuser`用户`READ`权限
AccessControlList acl = new AccessControlList();
CanonicalGrantee canonicalGrant = new CanonicalGrantee("newuser");
canonicalGrant.setDisplayName("newuser");
acl.grantPermission(canonicalGrant, Permission.Read);

//构造一个设置桶ACL的请求
SetBucketAclRequest sbar = new SetBucketAclRequest(buc, acl);

//设置桶的ACL
client.setBucketAcl(sbar);
```
