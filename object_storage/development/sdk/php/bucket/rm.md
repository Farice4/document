### 删除桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`delete_bucket`方法。

#### 示例

删除名叫my-bucket的桶。

```
//初始化
$client = ...

$key = "my-bucket";

$response = $client->delete_bucket($key);
echo $response->isOK();

//delete_bucket($key)是当my-bucket为空时才会删除成功，若bucket内有对象，想强制删除bucket，可以使用delete_bucket($key, 1)
```

输出：

```
1
```
