### 环境准备

参考 [这篇文档] (http://docs.aws.amazon.com/AWSSdkDocsNET/latest/V2/DeveloperGuide/net-dg-nuget.html), 安装 C# 的开发环境.

### 初始化

要使用易云云存储，需要使用账户自己的 `access_key` 和 `secret_key` 来进行认证并初始化一个 Python SDK 中的 `S3Connection` （ `boto.s3.connection` ） 实例，在这一 `S3Connection` 实例的基础上完成其它操作。下面是对一般操作的所需要进行的基础流程:

```
using System;
using Amazon;
using Amazon.S3;
using Amazon.S3.Model;

namespace s3
{
	class S3Demo
	{
        static string accessKey = "P470284NW2XX7EG9GGI1";
        static string secretKey = "CNdQ2r1IQQthi82dgrscCpq86KdgYpacwuTOcaWw";
        AmazonS3Client client;
        private void Init()
        {
            AmazonS3Config config = new AmazonS3Config();
            config.ServiceURL = "http://obs.eayun.com:9090";
            client = new AmazonS3Client(accessKey, secretKey, config);
        }
        void DoRequest()
        {
            // 具体的请求操作, 看详细的每个文档
        }
		public static void Main (string[] args)
		{
            S3Demo demo = new S3Demo();
            demo.Init();
            demo.DoRequest();
		}
	}
}
```
