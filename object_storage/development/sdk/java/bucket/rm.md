### 删除桶
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明

* 获得一个client实例。
* 调用client实例的`deleteBucket`方法。

#### 示例

删除名叫my-bucket的桶。

```
//初始化
AmazonS3 client = ...

//删除my-bucket
String key = "my-bucket";
client.deleteBucket(bucket);

//再查看桶列表
List<Bucket> buckets = client.listBuckets();
for (Bucket bucket : buckets) {
	System.out.println(bucket.getName() + "\t"
			+ StringUtils.fromDate(bucket.getCreationDate()));
}
```

输出为空。
