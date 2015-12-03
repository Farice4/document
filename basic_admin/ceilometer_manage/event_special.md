# 查看事件特征描述

> 列出事件类型特征信息

`ceilometer trait-description-list -e "event type"`

举例如下:

```
# ceilometer trait-description-list -e port.create.start
+------------+-----------+
| Trait Name | Data Type |
+------------+-----------+
| request_id | string    |
| service    | string    |
| tenant_id  | string    |
+------------+-----------+

```
其包括"Trait Name" , "Data Type" ,在执行ceilometer trait-list 时会使用"Trait Name"

> 列出特征事件信息

`ceilometer trait-list -e "event type" -t "trait name"`

举例如下：

```
# ceilometer trait-list -e compute.instance.create.end -t instance_id
+-------------+--------------------------------------+-----------+
| instance_id | ff926011-6fbc-4ae4-b2f4-172909faa15a | string    |
| instance_id | ff931621-9542-4113-bef0-ae843434d897 | string    |
| instance_id | ffaa5302-ec9e-4592-9f68-4387452c58ab | string    |
| instance_id | ffaf4309-f7d1-4643-8908-909ef4e53f03 | string    |
| instance_id | ffc1fc75-53d3-4f69-93ca-e4939e8268fc | string    |
| instance_id | ffc23aa5-9e47-459a-a704-62d0781517c1 | string    |
| instance_id | ffd334d0-38d3-49d7-bac4-da2588df5508 | string    |
| instance_id | ffecde2f-3b3c-4800-a232-81e664b54055 | string    |
+-------------+--------------------------------------+-----------+

```
