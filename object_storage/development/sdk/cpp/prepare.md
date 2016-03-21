### 环境准备

安装 [libs3] (http://libs3.ischo.com/index.html), 在linux下可使用包管理工具安装, 如在 ubuntu 平台下:
```
$ sudo apt-get install -y libs3-dev
```

### 初始化

要使用易云云存储，需要使用账户自己的 `access_key` 和 `secret_key` 来进行认证并初始化一个 Python SDK 中的 `S3Connection` （ `boto.s3.connection` ） 实例，在这一 `S3Connection` 实例的基础上完成其它操作。下面是对一般操作的所需要进行的基础流程:

```
#include "libs3.h"
#include <stdlib.h>
#include <iostream>
#include <fstream>

// 下面的变量定义了大部分操作都需要的公共信息: key, host 等
const char access_key[] = "P470284NW2XX7EG9GGI1";
const char secret_key[] = "CNdQ2r1IQQthi82dgrscCpq86KdgYpacwuTOcaWw";
const char host[] = "obs.eayun.com:9090";

// S3ResponseHandler 结构定义了请求的回调函数, responsePropertiesCallback 
// 定义接收到响应时进行的回调, responseCompleteCallback 定义了响应完成时进行的回调函数
S3Status responsePropertiesCallback(
    const S3ResponseProperties *properties,
    void *callbackData)
{
    return S3StatusOK;
}

static void responseCompleteCallback(
    S3Status status,
    const S3ErrorDetails *error,
    void *callbackData)
{
    return;
}

S3ResponseHandler responseHandler =
{
    &responsePropertiesCallback,
    &responseCompleteCallback
};

// 业务逻辑处理
int main(int argc, char *argv[])
{
    S3_initialize("s3", S3_INIT_ALL, host);
    // 初始化之后, 在这里处理具体的 bucket 和 object 操作
    S3_deinitialize();
    return 0;
}
```
