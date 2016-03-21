### 分段上传操作

在SDK中，支持两种方式来进行分段上传。
* 低级API：步骤类似REST API中的Object多段上传。可用于一下情况：
    * 需要暂停、恢复分段上传
    * 上传期间修改分段大小
    * 上传前不知道上传对象的大小 

* 高级API：用于简化分段上传的操作，可以设置高级选项，例如用于分段上传的分段大小或在并发上传时您想要使用的线程数，也可以设置可选的数据元属性、存储类或 ACL。

#### 低级API

##### 多段上传

流程：

* 获得一个client实例。
* 调用client实例的`initiateMultipartUpload`方法设置上传的基本信息，返回一个上传ID。
* 调用client实例的`uploadPart`方法上传每个段，并将每个段上传请求的响应保存在列表中。
* 调用client实例的`completeMultipartUpload`方法合并段。

示例：

以每段5M来分段上传本地文件/home/obs/large.pdf到桶my-bucket中。

```
import com.amazonaws.services.s3.model.InitiateMultipartUploadRequest;
import com.amazonaws.services.s3.model.InitiateMultipartUploadResult;
import com.amazonaws.services.s3.model.UploadPartRequest;
import com.amazonaws.services.s3.model.CompleteMultipartUploadRequest;

//初始化
AmazonS3 client = ...

String obj = "large.pdf";
String buc = "my-bucket";
String localobj = "/home/obs/large.pdf";
File f = new File(localobj);

//初始化一个List，用来保存段上传请求的响应
List<PartETag> partETags = new ArrayList<PartETag>();

//获取文件大小并设置分段大小
long contentLength = f.length();
long partSize = 5 * 1024 * 1024; // 设置每段5M

//构造一个多段上传的请求
InitiateMultipartUploadRequest initRequest = new InitiateMultipartUploadRequest(
				buc, obj);
//初始化一个多段上传任务
InitiateMultipartUploadResult initResponse = client.initiateMultipartUpload(initRequest);

//上传每个段
long filePosition = 0;//当前段指向文件的位置
for (int i = 1; filePosition < contentLength; i++) {
	//确定该段的大小（最后一格段大小小于5M）
	partSize = Math.min(partSize, (contentLength - filePosition));
	//构造该段上传请求
	UploadPartRequest uploadRequest = new UploadPartRequest()
		.withBucketName(buc).withKey(obj)
		.withUploadId(initResponse.getUploadId()).withPartNumber(i)
		.withFileOffset(filePosition).withFile(f)
		.withPartSize(partSize);

	partETags.add(client.uploadPart(uploadRequest).getPartETag());

	filePosition += partSize;
	}
	
//合并段
CompleteMultipartUploadRequest compRequest = new 
	                CompleteMultipartUploadRequest(buc, 
	                                               obj, 
	                                               initResponse.getUploadId(), 
	                                               partETags);
client.completeMultipartUpload(compRequest);
```

#####列出多段上传任务

列出正在进行上传的任务。

流程：

* 获得一个client实例。
* 调用client实例的`listMultipartUploads`方法。

示例：

```
import com.amazonaws.services.s3.model.ListMultipartUploadsRequest;
import com.amazonaws.services.s3.model.MultipartUploadListing;

//初始化
AmazonS3 client = ...

String buc = "my-bucket";

//构造一个请求
ListMultipartUploadsRequest allMultpartUploadsRequest = new ListMultipartUploadsRequest(buc);

MultipartUploadListing multipartUploadListing = conn
	.listMultipartUploads(allMultpartUploadsRequest);
	
//分别列出正在上传的任务的对象名字和任务ID
for (MultipartUpload mpu : multipartUploadListing.getMultipartUploads()) {
	System.out.println(mpu.getKey() + "\t" + mpu.getUploadId());
}

```
输出：

```
cirros.raw	2/bgDucEMLjTp9srog2-DUr9r9v1DvJBB
test	2/QcO-el_AOvQioP_1EIDZ8Df7c1WZO6n
```

#####中止多段上传

中止正在进行上传的任务，可以释放掉这些任务占用的资源。需要提供上传任务的ID、桶名和对象名。

流程：

* 获得一个client实例。
* 调用client实例的abortMultipartUpload方法。

示例：

```
import com.amazonaws.services.s3.model.AbortMultipartUploadRequest;

//初始化
AmazonS3 client = ...

String obj = "large.pdf";
String buc = "my-bucket";
String id = "2/bgDucEMLjTp9srog2-DUr9r9v1DvJBB";

//构造一个请求
AbortMultipartUploadRequest amur = new AbortMultipartUploadRequest(buc, obj, id);
	
//删除该任务
client.abortMultipartUpload(amur);
```

####高级API

##### 多段上传

流程：

* 获得一个client实例。
* 使用client创建一个TransferManager实例。
* 调用TransferManager实例的upload方法。

示例：

以每段5M来分段上传本地文件/home/obs/large.pdf到桶my-bucket中。

```
import com.amazonaws.services.s3.transfer.TransferManager;
import com.amazonaws.services.s3.transfer.Upload;

//初始化
AmazonS3 client = ...

String obj = "large.pdf";
String buc = "my-bucket";
String localobj = "/home/obs/large.pdf";
File f = new File(localobj);

TransferManager tm = new TransferManager(client);
Upload upload = tm.upload(buc, obj, f);
try {
	//可以中止或者等待上传完成
	upload.waitForCompletion();
	System.out.println("上传完成");
} catch (AmazonClientException amazonClientException) {
	System.out.println("上传失败");
	amazonClientException.printStackTrace();
} catch (Exception e) {
	System.out.println(e.getMessage());
}
```

输出：

```
上传成功
```

#####中止多段上传

中止在某个桶中在指定时间点前开始的多段上传任务，并且仍在进行的上传任务。

流程：

* 获得一个client实例。
* 调用client实例的abortMultipartUpload方法。

示例：

中止1天前开始上传并且现在还未结束的任务。

```
import com.amazonaws.services.s3.model.AbortMultipartUploadRequest;

//初始化
AmazonS3 client = ...

String buc = "my-bucket";

TransferManager tm = new TransferManager(client);

//构造一个时间点
int days = 1000 * 60 * 60 * 24;
Date oneDayAgo = new Date(System.currentTimeMillis() - days);
	
//中止任务
try {
	tm.abortMultipartUploads(buc, oneDayAgo);
} catch (AmazonClientException amazonClientException) {
	amazonClientException.printStackTrace();
}
```

#####跟踪分段上传进度

高级分段上传 API 提供了一个侦听接口（ProgressListener），以便在使用 TransferManager 类上传数据时跟踪上传进度。

流程：

在上传请求上注册一个侦听接口

示例：

```
import com.amazonaws.event.ProgressEvent;
import com.amazonaws.event.ProgressListener;

String obj = "large.pdf";
String buc = "my-bucket";
String localobj = "/home/yippee/large.pdf";
File f = new File(localobj);

TransferManager tm = new TransferManager(client);

//构造一个上传请求
PutObjectRequest request = new PutObjectRequest(buc, obj, f);


//注册侦听接口
request.setGeneralProgressListener(ProgressListener() {
	@Override
	public void progressChanged(ProgressEvent progressEvent) {
		System.out.println("Transferred bytes: "
				+ progressEvent.getBytesTransferred());
	}
});

//上传
Upload upload = tm.upload(request);

try {
	// Or you can block and wait for the upload to finish
	upload.waitForCompletion();
	System.out.println("上传成功");
} catch (AmazonClientException amazonClientException) {
	amazonClientException.printStackTrace();
} catch (Exception e) {
	System.out.println(e.getMessage());
}

```
输出：

```
Transferred bytes: 0
Transferred bytes: 8192
Transferred bytes: 8192
Transferred bytes: 8192
Transferred bytes: 8192
Transferred bytes: 8192
...
上传成功
```
