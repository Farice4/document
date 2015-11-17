# 上传AMI镜像

上传 AMI 镜像，除了镜像文件，还需要该镜像的 kernel 文件和 initrd 文件。

## 命令格式

```
# eayunstack manage ami --help
usage: eayunstack manage ami [-h] [--kernel-file KERNEL_FILE]
                             [--initrd-file INITRD_FILE]
                             [--imange-file IMAGE_FILE] [-n NAME]

AMI Image Management

optional arguments:
  -h, --help            show this help message and exit
  --kernel-file KERNEL_FILE
                        The path of kernel file you want to use
  --initrd-file INITRD_FILE
                        The path of initrd file you want to use
  --imange-file IMAGE_FILE
                        The path of image file you want to upload
  -n NAME, --name NAME  The AMI image name
```

## 上传一个 AMI 镜像

* 准备：

  * 镜像：eayun-ubuntu-12.04-x64-raw-v1.img
  * kernel 文件：vmlinuz-3.2.0-23-generic
  * initrd 文件：initrd.img-3.2.0-23-generic

* 操作：

    ```
    # eayunstack manage ami --kernel-file vmlinuz-3.2.0-23-generic \
    --initrd-file initrd.img-3.2.0-23-generic \
    --image-file eayun-ubuntu-12.04-x64-raw-v1.img \
    --name ubuntu-12.04

    [ INFO  ] Kernel file uploading...

    [ INFO  ] Kernel file upload successfully.

    [ INFO  ] Initrd file uploading...

    [ INFO  ] Initrd file upload successfully.

    [ INFO  ] AMI image uploading...

    [ INFO  ] AMI image upload successfully!

    +-----------------------+--------------------------------------+
    | Property              | Value                                |
    +-----------------------+--------------------------------------+
    | Property 'kernel_id'  | 522006d0-0beb-49d3-9e27-de9ea80643ab |
    | Property 'ramdisk_id' | 1a88b538-06a0-4712-9aaf-3412bd6a6e94 |
    | checksum              | 8df6af3535674e0db9f7c54b7d24cdad     |
    | container_format      | ami                                  |
    | created_at            | 2015-05-22T04:31:10                  |
    | deleted               | False                                |
    | deleted_at            | None                                 |
    | disk_format           | ami                                  |
    | id                    | d3abdcea-7dc8-4991-8f0a-1746ab04395e |
    | is_public             | True                                 |
    | min_disk              | 0                                    |
    | min_ram               | 0                                    |
    | name                  | ubuntu-12.04                         |
    | owner                 | 96b0c3103d7f4ad38144bb2573269e0a     |
    | protected             | False                                |
    | size                  | 2146435072                           |
    | status                | active                               |
    | updated_at            | 2015-05-22T04:32:03                  |
    | virtual_size          | None                                 |
    +-----------------------+--------------------------------------+
    [ INFO  ] Image protecting...
    [ INFO  ] Protected successfully.

    [ INFO  ] Image protecting...
    [ INFO  ] Protected successfully.
    ```

* 说明：

  * --kernel-file, --initrd-file, --image-file 是必要的参数，--name 是可选参数，如果没有指定，则使用 image-file 的名称作为镜像名称；
  * 此命令只能在 **Controller** 节点上执行。
  * 所上传的镜像是 public 的，如想修改，须使用 glance 命令行进行修改。
