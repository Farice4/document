### 上传对象
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`putObject`方法。

#### 示例

* 创建一个名叫hello.txt的对象到my-bucket桶内，hello.txt的内容自定义。

    ```
    //初始化
    AmazonS3 client = ...
    
    String obj = "hello.txt";
    String buc = "my-bucket";
    
    //获取输入流
    String content = "Hello World!";
    ByteArrayInputStream input = new ByteArrayInputStream(content.getBytes());
    
    //定义对象的元数据
    ObjectMetadata meta = new ObjectMetadata();
    
    //上传对象
    client.putObject(buc, obj, input, meta);
    ```

* 上传一个本地文件newFile.txt到my-bucket桶内。

    ```
    //初始化
    AmazonS3 client = ...
    
    String obj = "newFile.txt";
    String buc = "my-bucket";
    
    //获取文件
    String pathname = "/home/obs/newFile.txt";
    File f = new File(pathname);
    
    //定义对象的元数据
    ObjectMetadata meta = new ObjectMetadata();
    
    //上传对象
    client.putObject(buc, obj, f);
    ```
