zookeeper模块用于监控zookeeper的指标，
从这些指标中可以了解zookeeper的当前运行状态。
该模块使用mntr命令采集zookeeper指标，
详见[官方文档](http://zookeeper.apache.org/doc/current/zookeeperAdmin.html#sc_zkCommands)


###1. 配置文件
```
- module: zookeeper
  metricsets: ["mntr"]
  period: 10s
  hosts: ["localhost:2181"]
```
- `module` 采集模块为zookeeper
- `metricsets` 监控项，目前只有"mntr"
- `period` 采集间隔时间
- `hosts` 被采集的服务器列表


###2. 指标数据样例
```
{
    "@timestamp":"2017-12-18T09:28:38.182Z",
    "zookeeper":{
        "mntr":{
            "latency":{
                "avg":0,
                "min":0,
                "max":4536
            },
            "approximate_data_size":1223213,
            "server_state":"leader",
            "outstanding_requests":0,
            "num_alive_connections":4,
            "pending_syncs":0,
            "followers":2,
            "znode_count":15967,
            "watch_count":5,
            "version":"3.4.8--1, built on 02/06/2016 03:18 GMT",
            "synced_followers":2,
            "packets":{
                "sent":3032349,
                "received":3032355
            },
            "max_file_descriptor_count":10240,
            "open_file_descriptor_count":33,
            "ephemerals_count":19
        }
    }
}
```

###3. 指标含义
| 指标 | 含义 |
| :---| :--- |
| @timestamp | 时间戳 |
| zookeeper.mntr.version | zookeeper版本号 |
| zookeeper.mntr.server_state | 集群模式：leader，follower，standalone |
| zookeeper.mntr.znode_count | znode的数量 |
| zookeeper.mntr.watch_count | watch的数量 |
| zookeeper.mntr.num_alive_connections | 当前的连接数 |
| zookeeper.mntr.ephemerals_count | 临时znode的数量 |
| zookeeper.mntr.approximate_data_size | zookeeper数据的大致大小 |
| zookeeper.mntr.outstanding_requests | 等待完成的请求数 |
| zookeeper.mntr.latency.avg | 节点通讯平均延时，单位毫秒 |
| zookeeper.mntr.latency.min | 节点通讯历史最小延时，单位毫秒 |
| zookeeper.mntr.latency.max | 节点通讯历史最大延时，单位毫秒 |
| zookeeper.mntr.packets.received | 接受网络包数量 |
| zookeeper.mntr.packets.sent | 发送网络包数量 |
| zookeeper.mntr.followers | 当前host的follower数量（只有leader有此指标） |
| zookeeper.mntr.synced_followers | 已同步的follower数量（只有leader有此指标） |
| zookeeper.mntr.pending_syncs | 等待同步的follower数量（只有leader有此指标） |
| zookeeper.mntr.open_file_descriptor_count | 当前打开的文件句柄数量（只有Unix平台有此指标） |
| zookeeper.mntr.max_file_descriptor_count | 历史最大的打开的文件句柄数量（只有Unix平台有此指标） |





