# 对象操作

> #### 注意
>因为当前ceph版本桶不支持多版本功能，那么object API中的所有相关对象多版本，消息头中的x-amz-version-id等暂时忽略。

对象操作包括了如下：

* [上传对象](put.md)
* [复制对象](copy.md)
* [获取对象内容](get_content.md)
* [获取对象元数据](get_meta.md)
* [获取对象ACL](get_acl.md)
* [设置对象ACL](set_acl.md)
* [删除对象](delete.md)
* [初始化多段上传任务](init_part_upload.md)
* [多段上传](upload_part.md)
* [列出所有段](list_parts.md)
* [合并段](combine_parts.md)
* [删除多段上传任务](delete_part_upload.md)
