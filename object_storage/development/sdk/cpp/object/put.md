### 上传对象

#### 前提条件

用户拥有易云云存储系统提供的 access_key 及 secret_key 。

#### 流程说明

* 调用 **S3_initialize()** 初始化
* 创建需要上传的数据结构(e.g. put_object_callback_data)和相关的回调函数, 调用 **S3_put_object** 上传对象
* 完成请求调用 **S3_deinitialize()** 释放资源

#### 示例

将名为 hello.txt 的对象上传到名为 "sample_bucket" 的bucket中, 命名为 world.txt

相关公共回调函数和变量定义请参照 [环境准备](../prepare.md)
```
S3BucketContext bucketContext =
{
        host,
        "sample_bucket",
        S3ProtocolHTTP,
        S3UriStylePath,
        access_key,
        secret_key
};

#include <sys/stat.h>
typedef struct put_object_callback_data
{
    FILE *infile;
    uint64_t contentLength;
} put_object_callback_data;

// 传输文件的回调
static int putObjectDataCallback(int bufferSize, char *buffer, void *callbackData)
{
    put_object_callback_data *data = (put_object_callback_data *) callbackData;

    int ret = 0;

    if (data->contentLength) {
        int toRead = ((data->contentLength > (unsigned) bufferSize) ? (unsigned) bufferSize : data->contentLength);
        ret = fread(buffer, 1, toRead, data->infile);
    }
    data->contentLength -= ret;
    return ret;
}

S3PutObjectHandler putObjectHandler =
{
        responseHandler,
        &putObjectDataCallback
};

int main(int argc, char *argv[])
{
    S3_initialize("s3", S3_INIT_ALL, host);

    put_object_callback_data data;
    const char sample_file[] = "hello.txt";
    const char sample_key[] = "world.txt";
    struct stat statbuf;
    if (stat(sample_file, &statbuf) == -1) {
        fprintf(stderr, "\nERROR: Failed to stat file %s: ", sample_file);
        perror(0);
        exit(-1);
    }

    // 填充 put_object_callback_data 结构体.
    int contentLength = statbuf.st_size;
    data.contentLength = contentLength;

    if (!(data.infile = fopen(sample_file, "r"))) {
        fprintf(stderr, "\nERROR: Failed to open input file %s: ", sample_file);
        perror(0);
        exit(-1);
    }
    S3_put_object(&bucketContext, sample_key, contentLength, NULL, NULL, 
                  &putObjectHandler, &data);

    S3_deinitialize();
    
    return 0;
}
```
