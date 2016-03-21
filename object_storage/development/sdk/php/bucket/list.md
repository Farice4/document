### 查看桶列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`list_buckets`方法。

#### 示例

查看桶列表。

```
//初始化
$client = ...

$ListResponse = $client->list_buckets();
    $Buckets = $ListResponse->body->Buckets->Bucket;
    foreach ($Buckets as $Bucket) {
        echo $Bucket->Name . "\t" . $Bucket->CreationDate . "</br>";
    }
```

输出：

```
my-bucket	2015-12-08T06:59:52.000Z
```
