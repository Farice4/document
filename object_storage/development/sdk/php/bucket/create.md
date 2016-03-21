### 创建桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`create_bucket`方法。

#### 示例

创建一个名叫my-bucket的桶。

```
//初始化
$client = ...

$key = "my-bucket";

//创建桶，create_bucket方法需要指定一个region，这里使用AmazonS3::REGION_US_E1，
//因为AmazonS3::REGION_US_E1是一个常量，值为''
$response = $client->create_bucket($key, AmazonS3::REGION_US_E1);
echo $response->isOK();
```

输出：

```
1
```
