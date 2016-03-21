### 多段上传
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 初始化一个分段上传的任务，使用`initiate_multipart_upload`。
* 上传段，使用`upload_part`。
* 完成多段上传，使用`complete_multipart_upload`。

#### 示例

删除桶my-bucket中的hello.txt。

```
//初始化
$client = ...
    
define('MB', 1024 * 1024);

$obj = "large.pdf";
$bucket = "my-bucket";
$local_file = "large.pdf";

//初始化一个多段上传任务
$init_response = $client->initiate_multipart_upload($bucket, $obj, array(
    'acl' => AmazonS3::ACL_PUBLIC,
    )
);
 
//获取上传ID
$upload_id = (string) $init_response->body->UploadId;
echo 'init:'.$init_response->isOK();

//获取文件的段数，按照每段5M分割
$parts = $client->get_multipart_counts(filesize($local_file), 5*MB);

//上传
foreach ($parts as $i => $part)
{
    $client->batch()->upload_part($bucket, $obj, $upload_id, array(
        'expect'     => '100-continue',
        'fileUpload' => $local_file,
        'partNumber' => ($i + 1),
        'seekTo'     => (integer) $part['seekTo'],
        'length'     => (integer) $part['length'],
    ));
}
$up_responses = $client->batch()->send();
echo 'up:'.$up_responses->isOK();

//获取已上传的段信息
$parts = $client->list_parts($bucket, $obj, $upload_id);

//合并段，完成上传任务
$complete_response = $client->complete_multipart_upload($bucket, $obj, $upload_id, $parts);
echo 'complete:'.$complete_response->isOK();
```
输出：

```
init:1
up:1
complete:1
```
