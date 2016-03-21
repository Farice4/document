### 获取对象元数据
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`get_object_metadata`方法。

#### 示例
```
//初始化
$client = ...

$obj = "hello.txt";
$buc = "my-bucket";

$response = $s3->get_object_metadata($buc, $obj);

echo $response->isOK();
echo $response['ContentType'];
```

输出：
```
1
string(10) "text/plain"
```


