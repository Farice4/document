### 查看桶列表
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
* 获得一个client实例。
* 调用client实例的`listBuckets`方法。

#### 示例

查看桶列表。

```
...
//导包
import com.amazonaws.services.s3.model.Bucket;

//初始化
AmazonS3 client = ...

//查看桶列表
List<Bucket> buckets = client.listBuckets();
for (Bucket bucket : buckets) {
	System.out.println(bucket.getName() + "\t"
			+ StringUtils.fromDate(bucket.getCreationDate()));
}
```

输出
```
my-bucket	2015-11-17T09:30:03.000Z
```
