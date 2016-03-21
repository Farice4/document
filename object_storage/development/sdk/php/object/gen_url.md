### 生成对象下载链接
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`get_object_url`方法。

#### 示例

获取桶my-bucket中的hello.txt的下载链接。

```
//初始化
$client = ...
    
$obj = "hello.txt";
$buc = "my-bucket";

$url = $client->get_object_url($buc, $obj);
echo $url;
```
输出：
```
http://obs.eayun.com/my-bucket/hello.txt
```
