# PPTP VPN 的使用
在 EayunStack v1.1 中，新增了对 PPTP 类型的 VPN 功能的支持，以下介绍如何使用这个功能。

## 使用方法
1. 创建一个 vpnservice 时，指定该 vpnservice 的类型为 pptp
  * 首先查看 pptp 这个类型是否在 Neutron 中得到支持，执行如下命令能够看到有一个名为 pptp 的 `service_type` 为 VPN 的条目即为支持
    ```
    neutron service-provider-list
    ```
  * 创建 vpnservice 时执行类型为 pptp
    ```
    neutron vpn-service-create 创建vpnservice的参数 --provider pptp
    ```
2. 创建 PPTP Credential，使得用户能够使用该 Credential 连接至 vpnservice，
需要注意的是，同一个 PPTP Credential 能够同时连接至多个该租户下的 vpnservice 以便于进行管理，同时允许该 credential 连接的多个 vpnservice 通过创建时的 `--vpnservices` 参数提供
    ```
    neutron eayun-pptp-credential-create USERNAME PASSWORD [--vpnservices list=true vpnservice_id1 vpnservice_id2 ...]
    ```
    其中，
      * USERNAME 和 PASSWORD 是必须指定的参数，表示这个 PPTP Credential 的用户名和密码
      * `--vpnservices` 是可选参数，若不指定，则暂时无法使用该 PPTP Credential 连接至任何 vpnservice

3. 更新 PPTP Credential，需要注意更新密码与更新 vpnservices 可以同时进行
  * 更新 PPTP Credential 的密码
    ```
    neutron eayun-pptp-credential-update CREDENTIAL_ID --password NEW_PASSWORD
    ```
  * 更新 PPTP Credential 所对应的 vpnservice 的列表
    ```
    neutron eayun-pptp-credential-update CREDENTIAL_ID --vpnservices list=true new_vpnservice_id1 new_vpnservice_id2 ...
    ```
  * 清空 PPTP Credential 所对应的 vpnservice 的列表，同样会使得无法使用该 PPTP Credential 连接至任何 vpnservice
    ```
    neutron eayun-pptp-credential-update CREDENTIAL_ID --vpnservices action=clear
    ```

4. 删除 PPTP Credential
    ```
    neutron eayun-pptp-credential-delete CREDENTIAL_ID
    ```
5. 列出当前所有的 PPTP Credential
    ```
    neutron eayun-pptp-credential-list
    ```
6. 显示一个 PPTP Credential 的详细信息
    ```
    neutron eayun-pptp-credential-show CREDENTIAL_ID
    ```
