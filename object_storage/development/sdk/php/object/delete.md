### 删除对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`delete_object`方法。

#### 示例

删除桶my-bucket中的hello.txt。

```
//初始化
$client = ...
    
$obj = "hello.txt";
$buc = "my-bucket";

$response = $client->delete_object($buc, $obj);
echo $response->isOK();
```
输出：

```
1
```
