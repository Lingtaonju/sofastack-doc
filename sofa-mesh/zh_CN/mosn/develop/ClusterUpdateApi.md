# Cluster/Upstream动态更新实现介绍

+ 总体方案：
clustermanager 中的 `ClusterAdapter`适配器，用于接收“回传的数据”，并根据回传数据更新cluster以及cluster对应的host；
其中cluster的配置项中，具有 
`"auto_discovery":true`
用于标识是否支持动态的cluster添加

+ 具体流程：
配置中心回传数据时，registry模块会调用cluster的如下接口，做cluster以及对应的host更新：

```go
    TriggerClusterUpdate(serviceName string, hosts []v2.Host) 

```

registy resub的时候，也会调用如下的删除cluster接口

```go
TriggerClusterDel(serviceName string) 
```
+ 更新方法：
设计cluster与servicename之间的对应关系，当前为：serviceName与clusterName一致；
根据clusterName查看cluster是否存在，如果不存在，就创建cluster，并更新hosts；
如果cluster已经存在，只更新hosts；删除也类似