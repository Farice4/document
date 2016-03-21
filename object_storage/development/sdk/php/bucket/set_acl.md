### 设置桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`set_bucket_acl`方法。

#### 示例
```
//初始化
$client = ...

$key = "my-bucket";

//设置bucketACL为PRIVATE
$response = $s3->set_bucket_acl($key, AmazonS3::ACL_PRIVATE);
echo $response->isOK();
```

输出：

```
1
```

