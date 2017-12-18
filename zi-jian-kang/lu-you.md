router模块用于监控router系统的指标，
从这些指标中可以了解router系统的当前运行状态。
主要包括router系统的tps，未完成量，及处理量等。
（router是一个将agent采集的数据中转至数据处理系统的服务端系统）。

###配置文件
```
- module: router
  metricsets: ["stats"]
  period: 10s
  hosts: ["localhost:7799"]
```
- `module` 采集模块为router系统
- `metricsets` 监控项，目前只有"stats"
- `period` 采集间隔时间
- `hosts` 被采集的服务器列表


###指标数据样例
```
{
  "@timestamp": "2017-12-12T08:33:41.881Z",
  "router": {
    "stats": {
      "core": {
        "processed": {
          "total": 156092022,
          "tps": 78031.1
        },
        "buffer": {
          "output": 395,
          "input": 0
        }
      },
      "kafka": {
        "input": {
          "total": 14176316,
          "tps": 7086.7
        },
        "output": {
          "success": 14176251,
          "failed": 0
        },
        "topic": "topic-3"
      }
    }
  }
}
```

###指标含义
| 指标                              | 含义                        |
| :------------------------------- | :-------------------------- |
| @timestamp                        | 时间戳                      |
| router.stats.core.buffer.input    | 接收数据的等待处理队列的大小    |
| router.stats.core.buffer.output   | 输出数据的等待处理队列的大小    |
| router.stats.core.processed.total | 已处理的总量                 |
| router.stats.core.processed.tps   | 处理tps                     |
| router.stats.kafka.topic          | 数据的目标输出topic           |
| router.stats.kafka.input.total    | 指定topic的接收到的总量        |
| router.stats.kafka.input.tps      | 指定topic的tps               |
| router.stats.kafka.output.success | 指定topic的成功数量           |
| router.stats.kafka.output.failed  | 指定topic的失败数量           |
