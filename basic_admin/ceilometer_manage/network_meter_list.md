# 查看网络流量采集与故障排除 #
  当配置网络流量采集后，ceilometer 在pipeline.yaml中添加关于bandwidth的采集后，将通过消息队列获取采集的网络流量．


#### 网络流量查看 ####

* 查看meter-list中关于bandwidth的采集项

```
+----------------------------------------+------------+------------+-----------------------------------------------------------------------+----------------------------------+----------------------------------+
| Name                                   | Type       | Unit       | Resource ID                                                           | User ID                          | Project ID                       |
+----------------------------------------+------------+------------+-----------------------------------------------------------------------+----------------------------------+----------------------------------+
| bandwidth                              | delta      | B          | 056423a2-1075-4937-bf29-6695a566108d                                  | None                             | 7cddfae96a4e4b0d871de526c65ec1b8 |
| bandwidth                              | delta      | B          | 2e102c61-3f15-4837-bc9e-bb531ce3799c                                  | None                             | 0644b0d9525e4cf7824656c98e9a3ebf |
| bandwidth                              | delta      | B          | 8975addf-6318-4312-b351-cda4e15a13ee                                  | None                             | 0bdd890d46c94ced8f35bd2af125b7a6 |
| bandwidth                              | delta      | B          | 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8                                  | None                             | 0bdd890d46c94ced8f35bd2af125b7a6 |
| bandwidth                              | delta      | B          | b7636446-45cb-4a4b-b003-472a4b980eed                                  | None                             | 0644b0d9525e4cf7824656c98e9a3ebf |
| bandwidth                              | delta      | B          | bb3c4264-d34b-488a-b7ad-34152e3d9c09                                  | None                             | 0644b0d9525e4cf7824656c98e9a3ebf |
| bandwidth                              | delta      | B          | c56f17e0-17bf-408f-b78b-aa6efb7df3d3                                  | None                             | 7b8bc72da97b463182d1ed6666d4b323 |
| bandwidth                              | delta      | B          | edc71b82-e38a-4064-9107-cfa535218f0d                                  | None                             | 7cddfae96a4e4b0d871de526c65ec1b8 |
| cpu                                    | cumulative | ns         | 0845ad8e-2186-4e52-9ea0-934540a95635                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu                                    | cumulative | ns         | 65f87823-070d-48f3-9d0c-41f9a18b74c0                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu_util                               | gauge      | %          | 0845ad8e-2186-4e52-9ea0-934540a95635                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu_util                               | gauge      | %          | 12377864-8132-4ccb-976e-54c1ae907e3c                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu_util                               | gauge      | %          | 1301be0c-aa85-4f38-bb0f-d34a7b8579bb                                  | 2bc111a2b1fb4ca69815db30b025adae | 0b17869e691542f488b48f9752719dcb |
| cpu_util                               | gauge      | %          | 2de8e950-baab-42ea-8e99-1dc0f2af867b                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu_util                               | gauge      | %          | 2e1ec37d-5ab7-4f5f-827e-6ad15bc39bc5                                  | 36a648ae5cda4a93a9a579614553ddc5 | 23491200ec4c4a10aa253bb42f7cc867 |
| cpu_util                               | gauge      | %          | 32cdbb19-9c89-4202-b74a-d56731cc5993                                  | 1b791a800fb94e7a866f266c64e05866 | 0bdd890d46c94ced8f35bd2af125b7a6 |
| cpu_util                               | gauge      | %          | 3e8527c6-c8bd-4ea2-b2ef-2bf3e049fa9d                                  | 36ac533bc99c4fd883d10c77976089c6 | 0644b0d9525e4cf7824656c98e9a3ebf |
| cpu_util                               | gauge      | %          | 4b4c1364-dd9a-4a98-9ed7-7c60e80f7a11                                  | 2b06757e1b4e4a9498a6f855401abc30 | 5bbb8b0802e248f2a7f1b4f61a8e58fe |
+----------------------------------------+------------+------------+-----------------------------------------------------------------------+----------------------------------+----------------------------------+

```

备注：可以看到上表中的resource id 即为创建meter-label时的meter-label id, project id中的919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 即为创建neutron-meter-label时的--tenant-id参数．

* 查看基于某个resource_id的bandwidth采集值

```
[root@node-18 ~](controller)# ceilometer sample-list -m bandwidth -q "resource_id=919060b6-d1cc-4ea5-881b-5ab2cb3d06d8" -l 10
+--------------------------------------+-----------+-------+--------------+------+----------------------------+
| Resource ID                          | Name      | Type  | Volume       | Unit | Timestamp                  |
+--------------------------------------+-----------+-------+--------------+------+----------------------------+
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:26:08.915000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:25:08.871000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 2703882370.0 | B    | 2016-06-17T03:24:08.870000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:23:08.866000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:22:08.875000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:21:08.904000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:20:08.828000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:19:08.909000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:18:09.002000 |
| 919060b6-d1cc-4ea5-881b-5ab2cb3d06d8 | bandwidth | delta | 0.0          | B    | 2016-06-17T03:17:08.847000 |
+--------------------------------------+-----------+-------+--------------+------+----------------------------+

```

备注：如果需要知道resource id是该租户流量流入还是流出，需要执行`neutron meter-label-list`, `neutron-meter-label-rule-list`获取信息，通常在建meter-label时，会取一个能够区分的名字对应rule里面的网络流入(admin-in 对应ingress)或者网络流出(admin-out 对应egress)

* 网络流量统计时常见问题

当进行网络流量统计时，如对一个文件上传，统计的结果根据文件上传时间，可能出现需要求出统计之后才能与文件上传大小相等

网络流量统计的是经过租户路由的数据记录，因此在ceilometer　meter中可能看到bandwidth中的统计值为０


