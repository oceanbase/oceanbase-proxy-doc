# 连接管理

## 名词解释

* rs list: OceanBase 集群里面启动 Root Service 服务的几台机器的 IP 列表（一般是每个 zone 有一个）；

* idc list: OceanBase 集群中 idc 和 region 的映射表，由 ocp 提供，通过 config server 来获取

* server list：OceanBase 集群中所有的机器 IP 列表；

* table entry：某个表的所有副本所在的机器 IP 列表；

* all_dummy_entry：all_dummy 表的 table entry，也就是承载某个租户 units 的所有 OceanBase 机器的 IP 列表 （对于 sys 租户，all_dummy_entry 等同于 server list）；

* cluster info：proxy用来保存每个集群rs list和拉取 rs list使用的url 的结构体；

* cluster resource：proxy用来管理每个集群资源的结构体，里面包含一个异步刷新server/zone list的task、黑名单

* table cache：保存所有table entry的全局缓存，key是由table_name、database_name、tenant_name、cluster_name 四个部分组成；


1. 尝试读本地缓存的rs list, 如果本地有rs list，用本地的rs list更新一次 all_dummy_entry；如果本地没有，就去config server上拉一次 rs list，保存到对应的cluster info中，并且用rs list来build一个__all_dummy_entry；

2. 查 __all_virtual_zone_stat 表去校验cluster name

3. 查 __all_virtual_proxy_server_stat 表拉一次集群的 server list以及他们的状态信息

4. 从 config server 上拉一次当前集群的 idc list

5. 从 __all_virtual_proxy_system_variables表 拉一次全量的系统变量

6. 启动异步定时任务，每隔10～20s刷一次 __all_virtual_zone_stat 表和 __all_virtual_proxy_server_stat 表

