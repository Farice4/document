# multiattach 卷使用示例

## 创建虚拟机时直接挂载 multiattach 卷
1. 创建 multiattach 卷

    创建 multiattach 卷 m-a-001 ：
    ```
    # cinder create --name m-a-001 --allow-multiattach 1
    +---------------------------------------+--------------------------------------+
    |                Property               |                Value                 |
    +---------------------------------------+--------------------------------------+
    |              attachments              |                  []                  |
    |           availability_zone           |                 nova                 |
    |                bootable               |                false                 |
    |          consistencygroup_id          |                 None                 |
    |               created_at              |      2015-08-06T03:49:55.000000      |
    |              description              |                 None                 |
    |               encrypted               |                False                 |
    |                   id                  | 3f72623f-7530-4ed4-8310-4b1fa71e722a |
    |                metadata               |                  {}                  |
    |              multiattach              |                 True                 |
    |                  name                 |               m-a-001                |
    |         os-vol-host-attr:host         |                 None                 |
    |     os-vol-mig-status-attr:migstat    |                 None                 |
    |     os-vol-mig-status-attr:name_id    |                 None                 |
    |      os-vol-tenant-attr:tenant_id     |   765c42b5ef4743abb88eef36d061daec   |
    |   os-volume-replication:driver_data   |                 None                 |
    | os-volume-replication:extended_status |                 None                 |
    |           replication_status          |               disabled               |
    |                  size                 |                  1                   |
    |              snapshot_id              |                 None                 |
    |              source_volid             |                 None                 |
    |                 status                |               creating               |
    |                user_id                |   a5388ee18fee4dde89685d01a3e77a1e   |
    |              volume_type              |                 None                 |
    +---------------------------------------+--------------------------------------+
    ```

2. 准备镜像和 flavor

    ```
    # glance image-list
    +--------------------------------------+--------+-------------+------------------+----------+--------+
    | ID                                   | Name   | Disk Format | Container Format | Size     | Status |
    +--------------------------------------+--------+-------------+------------------+----------+--------+
    | 6a2c0338-3940-4735-870e-7b37a020ba43 | cirros | qcow2       | bare             | 13200896 | active |
    +--------------------------------------+--------+-------------+------------------+----------+--------+

    # nova flavor-create m1.micro auto 256 1 1
    +--------------------------------------+----------+-----------+------+-----------+------+-------+-------------+-----------+
    | ID                                   | Name     | Memory_MB | Disk | Ephemeral | Swap | VCPUs | RXTX_Factor | Is_Public |
    +--------------------------------------+----------+-----------+------+-----------+------+-------+-------------+-----------+
    | ee77e177-3b07-4988-82ef-1e723f42f29e | m1.micro | 256       | 1    | 0         |      | 1     | 1.0         | True      |
    +--------------------------------------+----------+-----------+------+-----------+------+-------+-------------+-----------+
    ```

3. 创建第一台虚拟机

    创建虚拟机时使用之前创建的 multiattach 卷：
    ```
    # nova boot --flavor m1.micro --image cirros --block-device-mapping vdb=3f72623f-7530-4ed4-8310-4b1fa71e722a:::0 vm-001
    +--------------------------------------+--------------------------------------------------+
    | Property                             | Value                                            |
    +--------------------------------------+--------------------------------------------------+
    | OS-DCF:diskConfig                    | MANUAL                                           |
    | OS-EXT-AZ:availability_zone          | nova                                             |
    | OS-EXT-SRV-ATTR:host                 | -                                                |
    | OS-EXT-SRV-ATTR:hypervisor_hostname  | -                                                |
    | OS-EXT-SRV-ATTR:instance_name        | instance-00000002                                |
    | OS-EXT-STS:power_state               | 0                                                |
    | OS-EXT-STS:task_state                | scheduling                                       |
    | OS-EXT-STS:vm_state                  | building                                         |
    | OS-SRV-USG:launched_at               | -                                                |
    | OS-SRV-USG:terminated_at             | -                                                |
    | accessIPv4                           |                                                  |
    | accessIPv6                           |                                                  |
    | adminPass                            | rWBLfwLu88ki                                     |
    | config_drive                         |                                                  |
    | created                              | 2015-08-06T04:00:11Z                             |
    | flavor                               | m1.micro (ee77e177-3b07-4988-82ef-1e723f42f29e)  |
    | hostId                               |                                                  |
    | id                                   | b864311d-a459-4170-904b-4fae1909b02b             |
    | image                                | cirros (6a2c0338-3940-4735-870e-7b37a020ba43)    |
    | key_name                             | -                                                |
    | metadata                             | {}                                               |
    | name                                 | vm-001                                           |
    | os-extended-volumes:volumes_attached | [{"id": "3f72623f-7530-4ed4-8310-4b1fa71e722a"}] |
    | progress                             | 0                                                |
    | security_groups                      | default                                          |
    | status                               | BUILD                                            |
    | tenant_id                            | 765c42b5ef4743abb88eef36d061daec                 |
    | updated                              | 2015-08-06T04:00:12Z                             |
    | user_id                              | a5388ee18fee4dde89685d01a3e77a1e                 |
    +--------------------------------------+--------------------------------------------------+
    ```

4. 创建第二台虚拟机

    创建虚拟机时使用之前创建的 multiattach 卷：
    ```
    # nova boot --flavor m1.micro --image cirros --block-device-mapping vdb=3f72623f-7530-4ed4-8310-4b1fa71e722a:::0 vm-002
    +--------------------------------------+--------------------------------------------------+
    | Property                             | Value                                            |
    +--------------------------------------+--------------------------------------------------+
    | OS-DCF:diskConfig                    | MANUAL                                           |
    | OS-EXT-AZ:availability_zone          | nova                                             |
    | OS-EXT-SRV-ATTR:host                 | -                                                |
    | OS-EXT-SRV-ATTR:hypervisor_hostname  | -                                                |
    | OS-EXT-SRV-ATTR:instance_name        | instance-00000003                                |
    | OS-EXT-STS:power_state               | 0                                                |
    | OS-EXT-STS:task_state                | scheduling                                       |
    | OS-EXT-STS:vm_state                  | building                                         |
    | OS-SRV-USG:launched_at               | -                                                |
    | OS-SRV-USG:terminated_at             | -                                                |
    | accessIPv4                           |                                                  |
    | accessIPv6                           |                                                  |
    | adminPass                            | p8tEPS34Tw2M                                     |
    | config_drive                         |                                                  |
    | created                              | 2015-08-06T04:24:33Z                             |
    | flavor                               | m1.micro (ee77e177-3b07-4988-82ef-1e723f42f29e)  |
    | hostId                               |                                                  |
    | id                                   | 8bb990e2-a5a0-4b12-b6ce-5f43ced9a083             |
    | image                                | cirros (6a2c0338-3940-4735-870e-7b37a020ba43)    |
    | key_name                             | -                                                |
    | metadata                             | {}                                               |
    | name                                 | vm-002                                           |
    | os-extended-volumes:volumes_attached | [{"id": "3f72623f-7530-4ed4-8310-4b1fa71e722a"}] |
    | progress                             | 0                                                |
    | security_groups                      | default                                          |
    | status                               | BUILD                                            |
    | tenant_id                            | 765c42b5ef4743abb88eef36d061daec                 |
    | updated                              | 2015-08-06T04:24:34Z                             |
    | user_id                              | a5388ee18fee4dde89685d01a3e77a1e                 |
    +--------------------------------------+--------------------------------------------------+
    ```

5. 测试 multiattach 卷

    在 vm-001 上对 multiattach 卷 m-a-001 进行格式化，挂载，并写入测试文件：
    ```
    $ mkdir /tmp/1
    $ sudo mkfs.ext3 /dev/vdb
    $ sudo mount /dev/vdb /tmp/1
    $ sudo chmod 777 /tmp/1
    $ echo "vm-001" > /tmp/1/1
    $ sudo umount /dev/vdb
    ```

    在 vm-002 上直接挂载（在 vm-001 已经对 multiattach 卷 m-a-001 进行了格式化），读取 vm-001 的测试文件，并写入测试文件：
    ```
    $ mkdir /tmp/1
    $ sudo mount /dev/vdb /tmp/1
    $ cat /tmp/1/1
    vm-001
    $ echo "vm-002" > /tmp/1/2
    $ sudo umount /dev/vdb
    ```

    在 vm-001 上重新挂载 multiattach 卷 m-a-001 ，读取 vm-002 的测试文件：
    ```
    $ sudo mount /dev/vdb /tmp/1
    $ cat /tmp/1/2
    vm-002
    $ sudo umount /dev/vdb
    ```

## 在已有虚拟机上附加 multiattach 卷
1. 创建 multiattach 卷

    创建 multiattach 卷 m-a-002：
    ```
    # cinder create --name m-a-002 --allow-multiattach 1
    +---------------------------------------+--------------------------------------+
    |                Property               |                Value                 |
    +---------------------------------------+--------------------------------------+
    |              attachments              |                  []                  |
    |           availability_zone           |                 nova                 |
    |                bootable               |                false                 |
    |          consistencygroup_id          |                 None                 |
    |               created_at              |      2015-08-06T03:50:14.000000      |
    |              description              |                 None                 |
    |               encrypted               |                False                 |
    |                   id                  | c6d8a766-0cee-4021-ae05-c74668725e3f |
    |                metadata               |                  {}                  |
    |              multiattach              |                 True                 |
    |                  name                 |               m-a-002                |
    |         os-vol-host-attr:host         |                 None                 |
    |     os-vol-mig-status-attr:migstat    |                 None                 |
    |     os-vol-mig-status-attr:name_id    |                 None                 |
    |      os-vol-tenant-attr:tenant_id     |   765c42b5ef4743abb88eef36d061daec   |
    |   os-volume-replication:driver_data   |                 None                 |
    | os-volume-replication:extended_status |                 None                 |
    |           replication_status          |               disabled               |
    |                  size                 |                  1                   |
    |              snapshot_id              |                 None                 |
    |              source_volid             |                 None                 |
    |                 status                |               creating               |
    |                user_id                |   a5388ee18fee4dde89685d01a3e77a1e   |
    |              volume_type              |                 None                 |
    +---------------------------------------+--------------------------------------+
    ```

2. 附加 multiattach 卷

    将 multiattach 卷 m-a-002 附加到 vm-001 ：
    ```
    # nova volume-attach vm-001 c6d8a766-0cee-4021-ae05-c74668725e3f /dev/vdc
    +----------+--------------------------------------+
    | Property | Value                                |
    +----------+--------------------------------------+
    | device   | /dev/vdc                             |
    | id       | c6d8a766-0cee-4021-ae05-c74668725e3f |
    | serverId | b864311d-a459-4170-904b-4fae1909b02b |
    | volumeId | c6d8a766-0cee-4021-ae05-c74668725e3f |
    +----------+--------------------------------------+
    ```

    将 multiattach 卷 m-a-002 附加到 vm-002 ：
    ```
    # nova volume-attach vm-002 c6d8a766-0cee-4021-ae05-c74668725e3f /dev/vdc
    +----------+--------------------------------------+
    | Property | Value                                |
    +----------+--------------------------------------+
    | device   | /dev/vdc                             |
    | id       | c6d8a766-0cee-4021-ae05-c74668725e3f |
    | serverId | 8bb990e2-a5a0-4b12-b6ce-5f43ced9a083 |
    | volumeId | c6d8a766-0cee-4021-ae05-c74668725e3f |
    +----------+--------------------------------------+
    ```

3. 测试 multiattach 卷
    在 vm-001 上对 multiattach 卷 m-a-002 进行格式化，挂载，并写入测试文件：
    ```
    $ mkdir /tmp/2
    $ sudo mkfs.ext3 /dev/vdc
    $ sudo mount /dev/vdc /tmp/2
    $ sudo chmod 777 /tmp/2
    $ echo "vdc-vm-001" > /tmp/2/1
    $ sudo umount /dev/vdc
    ```

    在 vm-002 上直接挂载（在 vm-001 已经对 multiattach 卷 m-a-002 进行了格式化），读取 vm-001 的测试文件，并写入测试文件：
    ```
    $ mkdir /tmp/2
    $ sudo mount /dev/vdc /tmp/2
    $ cat /tmp/2/1
    vdc-vm-001
    $ echo "vdc-vm-002" > /tmp/2/2
    $ sudo umount /dev/vdc
    ```

    在 vm-001 上重新挂载 multiattach 卷 m-a-002 ，读取 vm-002 的测试文件：
    ```
    $ sudo mount /dev/vdc /tmp/2
    $ cat /tmp/2/2
    vdc-vm-002
    $ sudo umount /dev/vdc
    ```
