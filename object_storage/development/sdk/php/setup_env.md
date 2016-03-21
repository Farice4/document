### 环境准备

SDK版本使用1.6.2。

### 初始化

要使用eayunOBS，需要使用账户的access_key和secret_key来进行认证。

当获取到access_key和secret_key之后，可通过以下方式进行初始化一个client实例：

```
<?php
require 'sdk-1.6.2/sdk.class.php';
define('AWS_KEY', '...');
define('AWS_SECRET_KEY', '...');
$HOST = 'obs.eayun.com';
$PORT = 9090;

//创建一个client实例
$client = new AmazonS3(array(
    'key' => AWS_KEY,
    'secret' => AWS_SECRET_KEY,
));
$client->set_hostname($HOST, $PORT);
$client->allow_hostname_override(false);

//禁止掉ssl
$client->disable_ssl();

//设置路径，如bucket.obs.eayun.com为obs.eayun.com/bucket
$client->enable_path_style();
```
