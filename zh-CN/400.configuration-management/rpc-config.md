# RPC 配置项总览

## rpc_listen_port
<!-- 全局 -->
`rpc_listen_port` 配置项用于设置 RPC 服务监听端口。

| 参数类型 |   整数类型      |
| 默认值   | 2885     |
| 取值范围 | (1024, 65536)  |
| 是否重启 ODP 生效 | 是  |

## rpc_support_key_partition_shard_request
<!-- 全局 -->
`rpc_support_key_partition_shard_request` 配置项用于控制是否将 key 分区拆解到全部分区进行转发。

| 参数类型 |   布尔类型      |
| 默认值   | False     |
| 取值范围 | <ul><li>True：将 key 分区拆解到全部分区进行转发。</li><li>False：不将 key 分区拆解到全部分区进行转发。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_srv_session_pool_inactive_timeout
<!-- 全局 -->
`rpc_srv_session_pool_inactive_timeout` 配置项用于设置同步模块中服务端连接的 inactive 过期时间。

| 参数类型 |   时间类型      |
| 默认值   | 30s     |
| 取值范围 | [0s, 1d]  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_force_srv_black_list
<!-- 全局 -->
`rpc_enable_force_srv_black_list` 配置项用于控制鉴权登录是否启用强制黑名单。

| 参数类型 |   布尔类型      |
| 默认值   | False     |
| 取值范围 | <ul><li>True：启用强制黑名单。</li><li>False：不启用强制黑名单。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_force_srv_black_list
<!-- 全局 -->
`rpc_force_srv_black_list` 配置项鉴权登录的强制黑名单，格式为 `ip:rpc_port`，多个 OBServer 节点之间使用英文分号（`;`）分隔，如 `10.10.10.1:2882;10.10.10.2:2882`。

| 参数类型 |   字符串类型      |
| 默认值   | 默认为空     |
| 取值范围 | 不涉及  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_direct_expire_route_entry
<!-- 全局 -->
`rpc_enable_direct_expire_route_entry` 配置项用于控制是否强制失效清理过期的路由信息（不再 DIRTY 过程）
<!-- 没看懂 -->
| 参数类型 |   布尔类型      |
| 默认值   | False     |
| 取值范围 | <ul><li>True：强制失效清理过期的路由信息。</li><li>False：不强制失效清理过期的路由信息。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_reroute
<!-- 全局 -->
`rpc_enable_reroute` 配置项用于控制是否开启 RPC 二次路由。

| 参数类型 |   布尔类型      |
| 默认值   | True     |
| 取值范围 | <ul><li>True：开启 RPC 二次路由。</li><li>False：不开启 RPC 二次路由。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_congestion
<!-- 全局 -->
`rpc_enable_congestion` 配置项用于控制是否开启 RPC congestion 过程。

| 参数类型 |   布尔类型      |
| 默认值   | False     |
| 取值范围 | <ul><li>True：开启 RPC congestion 过程。</li><li>False：不开启 RPC congestion 过程。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_parallel_handler
<!-- 全局 -->
`rpc_enable_parallel_handler` 配置项用于控制是否开启异步执行框架。

| 参数类型 |   布尔类型      |
| 默认值   | True     |
| 取值范围 | <ul><li>True：开启异步执行框架。</li><li>False：不开启异步执行框架。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_max_server_table_entry_num
<!-- 全局 -->
`rpc_max_server_table_entry_num` 配置项用于xxx

| 参数类型 |   整数类型      |
| 默认值   | 10     |
| 取值范围 | [1, )  |
| 是否重启 ODP 生效 | 否  |

## rpc_max_request_batch_size
<!-- 全局 -->
`rpc_max_request_batch_size` 配置项用于设置单次发送请求中最大的 batch 数量。

| 参数类型 |   整数类型      |
| 默认值   | 5     |
| 取值范围 | [1, )  |
| 是否重启 ODP 生效 | 否  |

## rpc_request_timeout
<!-- 全局 -->
`rpc_request_timeout` 配置项用于设置请求发送的默认超时时间，单位 us。
<!-- 确认下参数类型 -->

| 参数类型 |   整数类型      |
| 默认值   | 5000000     |
| 取值范围 | [0, )  |
| 是否重启 ODP 生效 | 否  |

## rpc_net_timeout_base
<!-- 全局 -->
`rpc_net_timeout_base` 配置项用于设置基准发送速率，即 1B 数据的发送时间，单位为 ms。最低带宽为 200Bps，若配置速率低于 200Bps，server net会超时。
<!-- server net 会超时 是啥意思 -->
| 参数类型 |   整数类型      |
| 默认值   | 5     |
| 取值范围 | [0, )  |
| 是否重启 ODP 生效 | 否  |

## rpc_server_net_invalid_time_us
<!-- 全局 -->
`rpc_server_net_invalid_time_us` 配置项用于控制 server net pending发送的时间间隔，单位 us，默认为 10s

| 参数类型 |   整数类型      |
| 默认值   | 10000000     |
<!-- | 取值范围 |    |--无取值范围 -->
| 是否重启 ODP 生效 | 否  |

## rpc_server_net_max_pending_request
<!-- 全局 -->
`rpc_server_net_max_pending_request` 配置项用于设置 server net pending 发送请求的数量。pending 时间内达到配置的数量，ODP 会认为 server net 已失效，需要断链处理。

| 参数类型 |   整数类型      |
| 默认值   | 10     |
<!-- | 取值范围 |    |--没有取值范围 -->
| 是否重启 ODP 生效 | 否  |

## rpc_enable_global_index
<!-- 全局 -->
`rpc_enable_global_index` 配置项用于设置是否开启 global index rpc 请求路由能力的支持。

| 参数类型 |   布尔类型      |
| 默认值   | False     |
| 取值范围 | <ul><li>True：开启 global index rpc 请求路由能力的支持。</li><li>False：不开启 global index rpc 请求路由能力的支持。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_enable_retry_request_info_log
<!-- 全局 -->
`rpc_enable_retry_request_info_log` 配置项用于控制是否打印 RPC 请求重试信息。

| 参数类型 |   布尔类型      |
<!-- | 默认值   | True     |---确认下是否正确 -->
| 取值范围 | <ul><li>True：打印 RPC 请求 retry 信息。</li><li>False：不打印 RPC 请求 retry 信息。</li></ul>  |
| 是否重启 ODP 生效 | 否  |

## rpc_request_timeout_delta
<!-- 全局 -->
`rpc_request_timeout_delta` 配置项用于设置 ODP 内部请求处理超时时间的 delta 值。

| 参数类型 |   时间类型      |
| 默认值   | 10ms     |
| 取值范围 | [0ms, 30s]  |
| 是否重启 ODP 生效 | 否  |

## rpc_period_task_interval
<!-- 全局 -->
`rpc_period_task_interval` 配置项用于设置 ODP 处理 RPC 周期任务的时间间隔，主要清理 client net/server net 等遗留的无效请求。

| 参数类型 |   时间类型      |
| 默认值   | 600s     |
| 取值范围 | [1ms, 1d]  |
| 是否重启 ODP 生效 | 否  |

## rpc_server_net_handler_expire_time
<!-- 全局 -->
`rpc_server_net_handler_expire_time` 配置项用于设置 rpc server net 空闲默认退出时间。

| 参数类型 |   时间类型      |
| 默认值   | 1d     |
<!-- | 取值范围 | [1s, 10d]  |---介绍有冲突，还有一个取值范围为 [0s,]  -->
| 是否重启 ODP 生效 | 否  |

## rpc_server_entry_expire_time
<!-- 全局 -->
`rpc_server_entry_expire_time` 配置项用于设置 rpc server entry 空闲默认退出时间。

| 参数类型 |   时间类型      |
| 默认值   | 1d     |
<!-- | 取值范围 | [1s, 10d]  |---介绍有冲突，还有一个取值范围为 [0s,]  -->
| 是否重启 ODP 生效 | 否  |
