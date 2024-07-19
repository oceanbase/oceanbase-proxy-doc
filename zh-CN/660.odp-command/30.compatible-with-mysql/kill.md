# KILL

## 描述

该语句用来终止一个会话。

  普通用户默认只能kill当前用户下所有session， 其他租户下session对其不可见，相同租户下其他用户的session的kill与否根据其当前租户权限确定.
    observer的系统租户SYS可以kill本租户内任意一个session，proxy的超级管理员root@proxysys可以kill所有cluster、tenant下的session

## 语法

```sql
KILL [CONNECTION | QUERY ] proceselist_id
```
<!-- 我们还有一个 KILL PROXYSESSION {cs_id | connection_id} ss_id 命令么，通过这个中志连接 -->
<!-- query 的作用是 终止正在进行的 SQL 查询么，那 proceselist_id 需要指定为什么 -->
## 参数介绍

| 参数  | 描述  |
|-------|------|
| KIll  |      |
| KILL CONNECTION  | 与不含修改符的 `KILL` 一样，可终止给定的 Client Session ID。  |
| proceselist_id  | 当前会话的 Client Session ID，该 ID 是会话在客户端中的唯一标识。可以通过 `SHOW PROCESSLIST` 或者 `SHOW FULL PROCESSLIST` 命令查询。  |
<!-- 这里的 proceselist_id 是指 cs_id 么 -->
<!-- 也可以通过 `SHOW PROXYSESSION` 查询对不，这三个命令里输出的 ID 都是一样的么，还是不一样，然后这里只能使用 ODP侧的 ID，或 OB 侧的 ID？-->
## 示例
<!-- 待确认更新 -->
1. 使用 root@sys 用户通过 ODP 连接登录 OceanBase 数据库

   ```shell
   obclient -h10.10.10.1 -uroot@sys#obdemo -P2883 -p -c -A
   ```

   此处仅为示例，您需根据实际情况进行修改，详细的连接操作指引可参见 [通过 OBClient 连接 OceanBase 租户（MySQL 模式）](https://www.oceanbase.com/docs/common-oceanbase-database-cn-1000000000508045)。

2. 查看客户端连接，获取 `cs_id`

   ```shell
   SHOW PROXYSESSION;
   ```

   输出如下，返回结果中的 `Id` 即为 `cs_id`。

   ```shell
   +----------------------+-------+----------+--------+------+----------------------+------+-------------+-------------------+-------------------+-------+-------+-----------+
   | proxy_sessid         | Id    | Cluster  | Tenant | User | Host                 | db   | trans_count | svr_session_count | state             | tid   | pid   | using_ssl |
   +----------------------+-------+----------+--------+------+----------------------+------+-------------+-------------------+-------------------+-------+-------+-----------+
   | 12402504630519660556 | 64940 | test420  | sys    | root | 100.xx.xx.xx:63882   | NULL |           0 |                 1 | MCS_ACTIVE_READER | 76286 | 76286 |         0 |
   +----------------------+-------+----------+--------+------+----------------------+------+------------- +-------------------+-------------------+-------+-------+-----------+
   1 row in set

   ```

3. 根据获取的 `cs_id` 获取 `ss_id`

   ```shell
   SHOW PROXYSESSION ATTRIBUTE 64940;
   ```

   输出如下。

   ```shell
   +----------------------------------+----------------------+----------------+
   | attribute_name                   | value                | info           |
   +----------------------------------+----------------------+----------------+
   | proxy_sessid                     | -6044239443189891060 | cs common      |
   | cs_id                            | 64940                | cs common      |
   | cluster                          | test3233             | cs common      |
   | tenant                           | sys                  | cs common      |
   | user                             | root                 | cs common      |
   | host_ip                          | 100.xx.xx.xx         | cs common      |
   | host_port                        | 63882                | cs common      |
   | db                               | NULL                 | cs common      |
   | total_trans_cnt                  | 0                    | cs common      |
   | svr_session_cnt                  | 1                    | cs common      |
   | active                           | true                 | cs common      |
   | read_state                       | MCS_ACTIVE_READER    | cs common      |
   | tid                              | 76286                | cs common      |
   | pid                              | 76286                | cs common      |
   | idc_name                         |                      | cs common      |
   | modified_time                    | 0                    | cs stat        |
   | reported_time                    | 0                    | cs stat        |
   | hot_sys_var_version              | 0                    | cs var version |
   | sys_var_version                  | 2                    | cs var version |
   | user_var_version                 | 0                    | cs var version |
   | last_insert_id_version           | 0                    | cs var version |
   | db_name_version                  | 0                    | cs var version |
   | server_ip                        | xx.xx.xx.xx          | last used ss   |
   | server_port                      | 2881                 | last used ss   |
   | server_sessid                    | 3221579563           | last used ss   |
   | ss_id                            | 16                   | last used ss   |
   | state                            | MSS_KA_CLIENT_SLAVE  | last used ss   |
   | transact_count                   | 2                    | last used ss   |
   | server_trans_stat                | 0                    | last used ss   |
   | hot_sys_var_version              | 0                    | last used ss   |
   | sys_var_version                  | 2                    | last used ss   |
   | user_var_version                 | 0                    | last used ss   |
   | last_insert_id_version           | 0                    | last used ss   |
   | db_name_version                  | 0                    | last used ss   |
   | is_checksum_supported            | 1                    | last used ss   |
   | is_safe_read_weak_supported      | 0                    | last used ss   |
   | is_checksum_switch_supported     | 1                    | last used ss   |
   | checksum_switch                  | 1                    | last used ss   |
   | enable_extra_ok_packet_for_stats | 1                    | last used ss   |
   +----------------------------------+----------------------+----------------+
   39 rows in set
   ```

4. 终止客户端连接上的服务端连接

   ```shell
   KILL PROXYSESSION 64940 16;
   ```
