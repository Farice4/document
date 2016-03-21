### 设置对象的ACL
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`set_object_acl`方法。

#### 示例

设置对象hello.txt为ACL_PUBLIC。

```
//初始化
$client = ...

$obj = "hello.txt";
$buc = "my-bucket";

$response = $client->set_object_acl($buc, $obj, AmazonS3::ACL_PUBLIC);
echo $response->isOK();
```

输出：

```
1
```
