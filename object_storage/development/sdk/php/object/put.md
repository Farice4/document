### 上传对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`create_object`方法。

#### 示例

* 创建一个名叫hello.txt的对象到my-bucket桶内，hello.txt的内容自定义。

    ```
    //初始化
    $client = ...

    $obj = "hello.txt";
    $buc = "my-bucket";

    $response = $client->create_object($buc, $obj, array(
        'body' => "hello world!",
    ));
    echo $response->isOK();
    ```

输出：

```
1
```

* 上传一个本地文件newFile.txt到my-bucket桶内。

    ```
    //初始化
    $client = ...

    $obj = "newFile.txt";
    $buc = "my-bucket";
    $local_file = "newFile.txt";

    $response = $client->create_object($buc, $obj, array(
        'fileUpload' => $local_file,
    ));
    echo $response->isOK();
    ```

输出：

```
1
```
