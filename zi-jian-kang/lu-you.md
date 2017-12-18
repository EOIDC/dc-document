###采集配置
```
- module: router
  metricsets: ["stats"]
  period: 10s
  hosts: ["localhost:7799"]
```

指标数据样例
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

指标含义