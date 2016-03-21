### 查看桶内对象的列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`get_object_list`方法。

#### 示例
```
//初始化
$client = ...

$key = "my-bucket";

//获取对象列表
$response = $s3->get_object_list($key);
echo $response->isOK();
```

输出：

```
1
```
