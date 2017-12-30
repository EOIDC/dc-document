# Server配置

采集的Server端分为cell和router两个子组件

## 配置cell

> $CELL_HOME表示cell被安装到的路径，比如/itoa/app/cell

`cell`的配置文件位于`$CELL_HOME/conf/cell.toml`，分为三大配置节`server`，`log`，`kafka`。一个完整的配置示例如下：

```
[server]
id = "1"
cluster_bind = "172.16.128.198:7777"
cluster_peers = ["172.16.128.198:7778","172.16.128.198:7779"]
tcp_bind = "172.16.128.141:7788"
dashboard_bind = "172.16.128.141:8080"

[log]
level = "DEBUG3"
max_count = 5
max_size = "10m"

[kafka]
brokers = ["localhost:9092"]
version = "0.10.0.0"
pipeline_topic_request = "ITOA_TO_CELL_REQ"
pipeline_topic_response = "ITOA_TO_CELL_RESP"
roundtrip_topic_request = "CELL_TO_ITOA_REQ"
roundtrip_topic_response = "CELL_TO_ITOA_RESP"
report_hw_topic = "CELL_REPORT"
```

### server

`server`用于配置服务本身的运行参数

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| id | Require |  | 作为集群部署cell的时候，这个id用于在集群中唯一标识一个cell节点 |
| cluster_bind | Option |  | 作为集群部署cell的时候是必要的，这个bind用于集群内部通信 |
| cluster_peers | Option |  | 作为集群部署cell的时候是必要的，这个bind用于选择集群中的种子节点，种子节点的配置通常是除自己以外的其他节点 |
| tcp_bind | Require |  | cell将监听这个ip和端口，agent会通过这个bind连接进来，注意，此处不能配置成0.0.0.0，必须指定某个网卡 |
| dashboard_bind | Require |  | cell将监听这个ip和端口，用于提供http服务，http服务包括页面和接口 |

### log

`log`用于配置日志策略

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| level | Option | INFO | 日志级别。在守护进程模式下将输出在$CELL_HOME/logs目录下 |
| max_count | Option | 5 | 日志最大个数。为了避免日志爆盘，日志文件个数可通过这个值限制，与之相对应的还有max_size配置 |
| max_size | Option | 10m | 每个日志文件的最大大小，可以写成10m, 0.1G等。参见max_count |

### kafka

`kafka`用于配置cell与kafka通信的参数，由于后端与cell通信主要采用kafka，这里的配置十分关键，需要跟后端匹配

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| brokers | Require |  | kafka集群节点。配置若干个kafka集群节点，注意必须是同一个集群 |
| version | Option | 0.10.0.0 | 使用何种版本的kafka接口api |
| pipeline_topic_request | Require | | 后端调用cell时候，请求消息使用的topic |
| pipeline_topic_response | Require | | 后端调用cell时候，cell响应使用的topic |
| roundtrip_topic_request | Require | | cell调用后端时候，请求消息使用的topic |
| roundtrip_topic_response | Require | | cell调用后端时候，后端相应使用的topic |
| report_hw_topic | Option | | agent硬件的心跳信息上报的topic |

## 配置router


> $ROUTER_HOME表示router被安装到的路径，比如/itoa/app/router

`router`的配置文件位于`$ROUTER_HOME/conf/config.toml`，分为五大配置节`core`，`log`，`server`，`kafka`，`export`。一个完整的配置示例如下：

```
[core]
buffer = 1000
procs = 4

[log]
max_count = 5
max_size = "10m"
level = "DEBUG3"

[server]
ip = "0.0.0.0"
port = 5044
ssl = false
protocol = "lumberjack.v2"
chan_size = 128

[kafka]
brokers = ["192.168.31.23:9092"]#,"192.168.31.93:9092","192.168.31.94:9092"]
version = "0.10.0.0"
topic_keys = ["@@topic"]
default_topic = "eoi_router_default"
async_producer = true
max_message_bytes = "1m"
channel_buffer_size = 1024
ack = 1

[export]
[export.http]
host = "0.0.0.0"
port = 7799
```

### core

`core`用于配置核心策略，往往影响性能

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| buffer | Option | 1000 | 缓冲池大小。router从采集器收集数据，将数据转发到kafka，作为转发器，当输入输出速度不对等的时候，缓冲池表明最大留存在router内存中的记录条数 |
| procs | Option |  | 同时开启的线程数，该数量不建议设置超过硬件核数，一般无需设置，系统会自动选择 |

### log

`log`用于配置日志策略

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| level | Option | INFO | 日志级别。在守护进程模式下将输出在$CELL_HOME/logs目录下 |
| max_count | Option | 5 | 日志最大个数。为了避免日志爆盘，日志文件个数可通过这个值限制，与之相对应的还有max_size配置 |
| max_size | Option | 10m | 每个日志文件的最大大小，可以写成10m, 0.1G等。参见max_count |

### server

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| ip | Require |  | bind的ip地址 |
| port | Require |  | bind的端口，采集器将通过ip:port将数据上报 |
| ssl | Option | false | 是否启用ssl传输，当启动ssl传输时，客户端也需要采用ssl传输。`$ROUTER/conf/server.crt`和`$ROUTER/conf/server.key`是使用的证书 |
| protocol | Option | lumberjack.v2 |  使用日志传输协议，这里目前支持`lumberjack.v2` |
| chan_size | Option | 128 | 前置数据接收时，每个连接可缓存的最大批次个数。如果连接的缓冲中有批次待处理，系统将每3s向采集器发送`keepalive`信号 |

### kafka

`kafka`用于配置router与kafka通信的参数

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| brokers | Require |  | kafka集群节点。配置若干个kafka集群节点，注意必须是同一个集群 |
| version | Option | 0.10.0.0 | 使用何种版本的kafka接口api |
| topic_keys | Option | ["@@topic"]| 对于上报的json消息，可配置多个topic搜索键，默认是`@@topic`。换句话说，默认router将搜索`@@topic`这个键对应的值，将消息转发进kafka集群的这个topic。由于可能多套系统共存，所以这个键是可以配置多个的 |
| default_topic | Option | eoi_router_default | 大概某条json消息，无法匹配到topic时，将数据转发到某个固定的topic |
| async_producer | Option | true | 采用异步生产，默认采用异步生产，可提高吞吐率 |
| max_message_bytes | Options | 1m | json消息的最大大小，这个配置建议与kafka的最大消息大小保持一致。默认是1m |
| channel_buffer_size | Option | 1024 | 在向kafka生产消息的时候，缓冲多少条消息。这可提高一些吞吐率，默认1024 |
| ack | Option | 1 | 在向kafka异步生产消息的时候，采用何种确认策略。0：无响应，1：等待kafka写本地，2：等待kafka写备份。这三种策略性能由高到低，安全性由低到高 |

### export

`export`配置路由如何提供性能监控接口。可以通过方为`http://export.http.host:export.http.port/api/stats`来查看路由的性能。