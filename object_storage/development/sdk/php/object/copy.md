### 复制对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`copy_object`方法。

#### 示例
```
//初始化
$client = ...

$obj = "hello.txt";
$buc = "my-bucket";

$des_obj = "hello-copy.txt";
$des_buc = "my-bucket";

//复制对象
$response = $s3->copy_object(
    array( // Source
      'bucket'   => $buc,
      'filename' => $obj
    ),
    array( // Destination
      'bucket'   => $des_buc,
      'filename' => $des_obj
    )
);

echo $response->isOK();
```

输出：
```
1
```
