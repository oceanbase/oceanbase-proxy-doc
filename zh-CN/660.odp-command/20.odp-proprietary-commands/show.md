# SHOW

## 描述

该语句用于展示 ODP 和 OceanBase 数据库信息。

## 语法

```sql
SHOW {
    PROXYINFO BINARY
   | PROXYINFO UPGRADE
   | PROXYINFO IDC
   | PROXYCLUSTER [IDC] [like 'cluster_name']
   | PROXYCONFIG [diff] [like 'config_name']
   | PROXYCONGESTION [all] [cluster_name]
   | PROXYMEMORY [objpool]
   | PROXYNET CONNECTION [thread_id [LIMIT xx]]
   | PROXYNET THREAD
   | PROXYROUTE [like 'cluster [tenant [db [table]]]']
   | PROXYSESSION
   | PROXYSESSION ATTRIBUTE [id [like 'xxx']]
   | PROXYSESSION STAT id [like 'xx']
   | PROXYSESSION VARIABLES [all] id [like 'xx']
   | PROXYSM [sm_id]
   | PROXYSTAT [refresh] [like 'xx']
   | PROXYTRACE [session_id [attempt_id]]
   | PROXYVIP [vid vip:vport]
   | WARNLOG [[[log_id], thread_id], time]

--    | WARNLOG [[[log_id], thread_id], time]---没有看懂这个参数的选择情况

};
```
<!-- 还剩 SQL Audit 没有看懂啥意思 -->

alter proxyconfig set key=value
## 参数解释

| 参数   | 描述    |
|--------|--------|
| PROXYINFO BINARY | 以富文本形式展示 ODP binary 信息，包括版本、打包时间、MD5 等信息。 |
| PROXYINFO UPGRADE | 以富文本形式展示 ODP 自身运行/升级相关信息。 |
| PROXYINFO IDC | 以富文本形式展示 ODP IDC 匹配信息。 |
| PROXYCLUSTER [IDC] [like 'cluster_name'] | 展示集群的 rs_list 详细信息，支持 like 模糊匹配（支持 `%` 和 `_`）。配置 `IDC` 后展示集群的 idc_list 详细信息。  |
| PROXYCONFIG [diff] [like 'config_name'] | 展示 ODP 的配置项，配置 `diff` 后仅展示和默认参数不一样的配置项。支持 like 模糊匹配（支持 `%` 和 `_`）。 |
| PROXYCONGESTION [all] [cluster_name] |  |
| PROXYMEMORY [objpool] |  |
| PROXYNET CONNECTION [thread_id [LIMIT xx]] |  展示 ODP 当前各个连接的属性状态。可通过配置 `thread_id` 展示指定线程上的各连接状态，不指定时默认展示 ODP 全部连接的属性状态。指定 `thread id` 时，支持 `LIMIT [offset,] rows` 和 `LIMIT rows OFFSET offset`，格式与 MySQL 完全兼容，并且当 rows 设置为 `-1` 时，表示展示全部行。  |
| PROXYNET THREAD | 展示 ODP 内部各个工作线程属性状态。ODP 工作线程个数由 [work_thread_num](../../400.configuration-management/200.global-configuration-items/2570.work_thread_num.md) 控制。 |
| PROXYROUTE [like 'cluster [tenant [db [table]]]'] | 展示 ODP 内部 table entry 状态，默认展示所有 table entry 状态，支持 like 模糊匹配（支持 `%` 和 `_`）。ODP 内部 SQL 路由地址信息以 table entry 为单位，每个 table entry 由集群名、租户名、数据库名和表名组成，不指定表名的 SQL，内部使用 `__all_dummy` 表标记。 |
| PROXYSESSION | 展示当前 ODP 上租户连接的全部 Client Session 的内部状态，与 `SHOW PROCESSLIST` 命令不同的是，`SHOW PROXYSESSION` 可以展示所有租户下 Client Session 的状态，并包括每个租户的所在的集群。  |
<!-- SHOW PROCESSLIST 命令也能查看所有租户呀，验证下
而且查看所有租户的要求是要 root@sys 执行吧，普通租户执行只能查看当前租户的 Client Session 状态 -->
<!-- 验证下当 cd_id 一致的那个配置项配置后，输出的 proxy_sessid 和 id 是否一致 -->
| PROXYSESSION ATTRIBUTE [id [like 'xxx']] | 展示指定 Client Session 的详细内部状态，包括该 Client Session 上涉及的相关 Server Session。<ul><li>不指定 <code>id</code> 时，显示当前 Session 的详细状态（ODP 1.1.0 版本起开始支持），支持模糊查询当前 Session 指定属性名称的 value（ODP 1.1.2 版本起开始支持）。</li><li>指定 <code>id</code> 时， 支持模糊查询指定属性名称的 value（ODP 1.1.0 版本起开始支持）。</li><li><code>id</code> 既可以是 <code>cs_id</code>，也可以是 <code>connection_id</code>，显示结果相同。</br><code>cs_id</code> 为 ODP 内部标记的每个 Client 的 ID 号，<code>connection_id</code> 为整个 OceanBase 数据库标记的每个 Client 的 ID 号。MySQL 模式下的 <code>connection_id</code> 通过 <code>SELECT CONNECTION_ID();</code> 语句获取，Oracle 模式下的 <code>connection_id</code> 通过 <code>SHOW FULL PROCESSLIST;</code> 语句获取。</li><li>like 模糊匹配，支持 <code>%</code> 和 <code>_</code>。</li></ul> |
| PROXYSESSION STAT id [like 'xx'] | 展示 ODP 指定 Client Session 的内部统计项（包括：SQL 请求响应数量、SQL 请求响应大小等）。<ul><li><code>id</code> 既可以是 <code>cs_id</code>，也可以是 <code>connection_id</code>，显示结果相同。</br><code>cs_id</code> 为 ODP 内部标记的每个 Client 的 ID 号，<code>connection_id</code> 为整个 OceanBase 数据库标记的每个 Client 的 ID 号。MySQL 模式下的 <code>connection_id</code> 通过 <code>SELECT CONNECTION_ID();</code> 语句获取，Oracle 模式下的 <code>connection_id</code> 通过 <code>SHOW FULL PROCESSLIST;</code> 语句获取。</li><li>like 模糊匹配，支持 <code>%</code> 和 <code>_</code>。</li></ul>  |
| PROXYSESSION VARIABLES [all] id [like 'xx'] | 展示指定 Client Session 的 Session 变量。<ul><li>不配置 <code>all</code> 参数时，展示指定 Client Session 的本地 Session 变量（包括：修改过的系统变量和用户变量）。</li><li>配置 <code>all</code> 参数时，展示指定 Client Session 的全部 Session 变量（包括：所有系统变量和用户变量）。</li><li><code>id</code> 既可以是 <code>cs_id</code>，也可以是 <code>connection_id</code>，显示结果相同。</br><code>cs_id</code> 为 ODP 内部标记的每个 Client 的 ID 号，<code>connection_id</code> 为整个 OceanBase 数据库标记的每个 Client 的 ID 号。MySQL 模式下的 <code>connection_id</code> 通过 <code>SELECT CONNECTION_ID();</code> 语句获取，Oracle 模式下的 <code>connection_id</code> 通过 <code>SHOW FULL PROCESSLIST;</code> 语句获取。</li><li>like 模糊匹配，支持 <code>%</code> 和 <code>_</code>。</li></ul> |
| PROXYSM [sm_id] | 以富文本形式展示 ODP 各个 StateMachine 内部状态，支持展示指定 StateMachine 的内部状态。 |
| PROXYSTAT [refresh] [like 'xx'] |  |
| PROXYTRACE [session_id [attempt_id]] | 展示指定连接上任意一次尝试路径中的状态，不配置任何参数时默认展示当前连接所有尝试路径，不配置 `attempt_id` 时默认展示指定连接上的所有尝试路径。ODP 内部对每个事务的执行都会执行若干次路由尝试，每次路由都会经历选择 OBServer 节点、黑名单检测、同步状态、发送 SQL 四步。使用该命令查看尝试路径状态前需开启配置项 [enable_trace_stats](../../400.configuration-management/200.global-configuration-items/920.enable-trace-stats.md)，可通过 `alter proxyconfig set enable_trace_stats=true` 命令开启。 |
| PROXYVIP [vid vip:vport] |  |
| WARNLOG [[[log_id], thread_id], time] |  |
