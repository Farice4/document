### 环境准备
1. Perl 开发环境。

2. 安装 `Amazon::S3` Perl 模块及其所依赖的其他模块。

### 初始化

要使用易云云存储，需要使用账户自己的 `access_key` 和 `secret_key` 来进行认证并初始化一个 Perl SDK 中的 `Amazon::S3` 实例，在这一 `Amazon::S3` 实例的基础上完成其它操作。

```
#!/usr/bin/perl -w
use strict;
use Amazon::S3;

my $access_key = '9FSTXQR54VKR7VEM7GAJ';
my $secret_key = 'TRYrqiFTwIC8h6hw1jFQKd5o4RHfzIsjje4qawOG';

my $conn = Amazon::S3->new({
    aws_access_key_id => $access_key,
    aws_secret_access_key => $secret_key,
    host => 'obs.eayun.com',
    secure => 0,
});
```
