### 获取对象内容
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`get_object`方法。

#### 示例

* 获取桶my-bucket中的hello.txt的内容。

    ```
    //初始化
    $client = ...
    
    $obj = "hello.txt";
    $buc = "my-bucket";
    
    $response = $client->get_object($buc, $obj);
    echo $response->isOK();
    ```
    输出：
    ```
    1
    ```
* 下载桶my-bucket中的hello.txt到本地。

    ```
    //初始化
    $client = ...
    
    $obj = "hello.txt";
    $buc = "my-bucket";
    $local_file = "hello.txt";
    
    $response = $client->get_object($buc, $obj,
        'fileDownload' => $local_file
    );
    echo $response->isOK();
    ```
    输出：
    ```
    1
    ```
