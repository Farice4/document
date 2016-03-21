### 环境准备

1. Java环境使用jdk1.6以上。

2. SDK使用AWS SDK For Java。
    
    使用maven构建：
    
    ```
    <dependency>
    	<groupId>com.amazonaws</groupId>
    	<artifactId>aws-java-sdk</artifactId>
    	<version>1.10.34</version>
    </dependency>
    ```

### 初始化

要使用eayunOBS，需要使用账户的access_key和secret_key来进行认证。

当获取到access_key和secret_key之后，可通过以下方式进行初始化一个client实例：

```
import com.amazonaws.auth.AWSCredentials;
import com.amazonaws.auth.BasicAWSCredentials;
import com.amazonaws.ClientConfiguration;
import com.amazonaws.services.s3.AmazonS3Client;
import com.amazonaws.services.s3.AmazonS3;

String accessKey = "...";
String secretKey = "...";

AWSCredentials credentials = new BasicAWSCredentials(accessKey,
		secretKey);

ClientConfiguration clientConfig = new ClientConfiguration();
clientConfig.setProtocol(Protocol.HTTP);
clientConfig.setSignerOverride("S3SignerType");//设置使用V2认证

AmazonS3 client = new AmazonS3Client(credentials, clientConfig);
client.setEndpoint("obs.eayun.com:9090");
```
