# 查看用户

### 通过Web horizon查看用户

* 登录Web horizon界面----点击identity-----点击用户，列出用户信息

![User_List](../Picture/user_list1.jpg)

* 点击编辑，可对用信息进行编辑（编辑内容包，用户描述信息、密码、项目）

![User_List](../Picture/user_list2.jpg)

* 如不需要编辑，点击取消

### 通过命令查看用户

* 查看用户，执行如下命令

> ```keystone user-list```(查看）

> ```keystone user-update```（用户修改）
###示例

```
 keystone user-list
+----------------------------------+---------------------------------------------------+---------+------------------------------------------------------------+
|                id                |                        name                       | enabled |                           email                            |
+----------------------------------+---------------------------------------------------+---------+------------------------------------------------------------+
| fb873958e4cc4a80947a2633d67a9550 |                      Admin123                     |   True  |                  eayun_display@eayun.com                   |
| a385d73636c84a29bbfc72a0b15d882c |                       admin                       |   True  |                      admin@localhost                       |
| 90bcd94d2f4941bc929a8ae33c5afe19 |                       apporc                      |   True  |                                                            |
| 63d5c2e694b64e0e8cd22b3a9b431fa8 |                       blkart                      |   True  |                                                            |
| 4b56388e443b4b90b30a41ef36c57d8b |                     ceilometer                    |   True  |                    ceilometer@localhost                    |
| bf5400a43366473095799da3e4ad9274 |                      chenyan                      |   True  |                                                            |
| 421b2538109e4097b6017e2ddc4acc8a |                       cinder                      |   True  |                      cinder@localhost                      |
| df39b9792856480b85b3264c3e847823 |                       coffee                      |   True  |                                                            |
| 4e097705e70443baa1eae2b5b4a5ef2d |                    coffee-user                    |   True  |                                                            |
| 8a4c7d9afdcd4730ac12e5dddf384639 |                    coffee_curl                    |   True  |                                                            |
| 0d32bd9690204e5ba7bd18446f974728 |                   coffee_curl_01                  |   True  |                                                            |
| 67f0346823f44d9c8c420776f393d0a2 |                   coffee_curl_02                  |   True  |                                                            |
| eb885843ae1744f9803fefc2000b623a | ctx_rally_2fa86d34e8774c3c8e4e1fdf72a9fc7f_user_3 |   True  | ctx_rally_2fa86d34e8774c3c8e4e1fdf72a9fc7f_user_3@email.me |
| b5358ab36fb04e8fb4a35f40f5424061 | ctx_rally_32aee12f2bf04525ab52e389ca3f79ad_user_0 |   True  | ctx_rally_32aee12f2bf04525ab52e389ca3f79ad_user_0@email.me |
| 4dc1e9fa144d4a2784ae10f7e38b603f | ctx_rally_39e2cef0ca7a4d0ebca45771c207e1cc_user_0 |   True  | ctx_rally_39e2cef0ca7a4d0ebca45771c207e1cc_user_0@email.me |
| 8e896a56d227437f834b927fe3f5c61a | ctx_rally_3d3d5e5fd3f94131ba3215e61e874a02_user_0 |   True  | ctx_rally_3d3d5e5fd3f94131ba3215e61e874a02_user_0@email.me |
| 5e09a6ce5e41437488736da81e9fd868 | ctx_rally_3d3d5e5fd3f94131ba3215e61e874a02_user_1 |   True  | ctx_rally_3d3d5e5fd3f94131ba3215e61e874a02_user_1@email.me |
| f74988477a4741d18dc10066f9980ed8 | ctx_rally_6b1ab172619d4c259702a4c2037dd8d0_user_2 |   True  | ctx_rally_6b1ab172619d4c259702a4c2037dd8d0_user_2@email.me |
| 0760b8ce2db742438302f8f0208fd563 | ctx_rally_6e2e4b6cc548426c908740a7c0622c8c_user_2 |   True  | ctx_rally_6e2e4b6cc548426c908740a7c0622c8c_user_2@email.me |
| 3d1f869857084a0b9757628852ee0cb1 | ctx_rally_72922e88412941ad8ba78687361821ea_user_0 |   True  | ctx_rally_72922e88412941ad8ba78687361821ea_user_0@email.me |
| 5d1b83480dbb41038526594bbfd95c48 | ctx_rally_72922e88412941ad8ba78687361821ea_user_1 |   True  | ctx_rally_72922e88412941ad8ba78687361821ea_user_1@email.me |
| 08588f23820540cf9d2e543c8ac1ebb5 | ctx_rally_72922e88412941ad8ba78687361821ea_user_4 |   True  | ctx_rally_72922e88412941ad8ba78687361821ea_user_4@email.me |
| 41c9390ef0d14690b04c2d0b48d6c680 | ctx_rally_85abb57b9c8143fba30b60a7cdd12c9b_user_0 |   True  | ctx_rally_85abb57b9c8143fba30b60a7cdd12c9b_user_0@email.me |
| 5c7bffdf9af64cc5be0eb76cd8238914 | ctx_rally_85abb57b9c8143fba30b60a7cdd12c9b_user_2 |   True  | ctx_rally_85abb57b9c8143fba30b60a7cdd12c9b_user_2@email.me |
| 6c4de12edce047378d5e6a442ad9ec0e | ctx_rally_b17a1ad3bf7a421a80681f2de31ca23a_user_0 |   True  | ctx_rally_b17a1ad3bf7a421a80681f2de31ca23a_user_0@email.me |
| 09fe2e4e8c3344dba4a1262f62466f63 | ctx_rally_df663ff012af4dd0bb3993d1e47f81ad_user_2 |   True  | ctx_rally_df663ff012af4dd0bb3993d1e47f81ad_user_2@email.me |
| 4f7518dd705742a88533155153b70dc9 | ctx_rally_ef2734b44a5d412c805c965ea13d696a_user_3 |   True  | ctx_rally_ef2734b44a5d412c805c965ea13d696a_user_3@email.me |
| 1f500364cecb48b49d299ed1051ded85 |                       cybing                      |   True  |                                                            |
| 06133e314c5b433993810f062d9e2543 |                      dunrong                      |   True  |                                                            |
| f4311a89d4bc4412881ba6aa7edbc255 |                       glance                      |   True  |                      glance@localhost                      |
| 6e8107463d6444c59fb0582106e214f8 |                        heat                       |   True  |                       heat@localhost                       |
| d09ba4a59eb24ffbb692a76e2949d0dd |                      heat-cfn                     |   True  |                     heat-cfn@localhost                     |
| 61146679afb7401f9f03f5e37f027adf |                       huntxu                      |   True  |                                                            |
| 37a20f05686b453394965727a6d77f88 |                      neutron                      |   True  |                     neutron@localhost                      |
| 08262b3cdda840e4a16abffb8d1de9ec |                        nova                       |   True  |                       nova@localhost                       |
| 202e643e43e5452a90f2bd77474dfc20 |                        root                       |   True  |                                                            |
| 4431259d0bae4a6b9f46abe8dab85e18 |                      suizhen                      |   True  |                     zhen.sui@eayun.com                     |
| 8c9e6f9ea8814cd98a35d271798b0330 |                 tmp_neutron_admin                 |   True  |                                                            |
| 50d2476fe8ca4fd3894add11bef6316f |                      zhaochao                     |   True  |                                                            |
+----------------------------------+---------------------------------------------------+---------+------------------------------------------------------------+
```

