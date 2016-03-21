### 设置桶的访问控制权限
#### 前提条件
用户拥有易云云存储系统提供的 `access_key` 及 `secret_key` 。

#### 流程说明
1. 初始化 `Amazon::S3` 实例；
2. 调用 `bucket` 方法获取桶对象；
3. 调用 `set_acl` 方法设置桶的访问控制权限。

#### 示例
```
# 初始化 Amazon::S3 实例
my $conn = ...

# 获取桶对象
my $bucket = $conn->bucket('my-new-bucket');

# 设置桶的访问控制权限
my $acl_xml = << 'END_ACL_XML';
<AccessControlPolicy xmlns="http://s3.amazonaws.com/doc/2006-03-01/">
  <Owner>
    <ID>zc-test-1</ID>
    <DisplayName>zc-test-1</DisplayName>
  </Owner>
  <AccessControlList>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>zc-test-1</ID>
        <DisplayName>zc-test-1</DisplayName>
      </Grantee>
      <Permission>FULL_CONTROL</Permission>
    </Grant>
    <Grant>
      <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
        <ID>zc-test-2</ID>
        <DisplayName>zc-test-2</DisplayName>
      </Grantee>
      <Permission>READ</Permission>
    </Grant>
  </AccessControlList>
</AccessControlPolicy>
END_ACL_XML

$bucket->set_acl({acl_xml=>$acl_xml});
```
