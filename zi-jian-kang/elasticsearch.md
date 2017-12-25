Elasticsearch模块用于监控Elasticsearch的指标，
从这些指标中可以了解Elasticsearch的当前运行状态。


###1. 配置文件
```
- module: elasticsearch
  metricsets: ["node_stats", "cluster_stats"]
  period: 10s
  hosts: ["localhost:9200"]
```


###2. 指标数据样例
####2.1 cluster_stats 集群指标
```
{
    "@timestamp":"2017-12-22T10:14:31.670Z",
    "elasticsearch":{
        "cluster":{
            "stats":{
                "status":"green",
                "indices":{
                    "count":1021,
                    "shards":{
                        "total":4554
                    },
                    "docs":{
                        "count":199968167
                    },
                    "segments":{
                        "count":10879
                    }
                },
                "nodes":{
                    "count":{
                        "total":5
                    },
                    "jvm":{
                        "mem":{
                            "heap":{
                                "used":18119515840,
                                "max":42382786560
                            }
                        }
                    },
                    "fs":{
                        "total":{
                            "bytes":1584643768320
                        },
                        "free":{
                            "bytes":1420238610432
                        },
                        "available":{
                            "bytes":1339625152512
                        }
                    }
                },
                "timestamp":1513937672209
            },
            "name":"elasticsearch-dev5.4"
        }
    }
}
```

####2.2 node_stats 节点指标
```
{
    "@timestamp":"2017-12-22T10:00:08.123Z",
    "elasticsearch":{
        "node":{
            "id":"deqrptrtS2yJCPl_OHxdTw",
            "name":"node-192.168.31.35",
            "host":"192.168.31.35",
            "stats":{
                "fs":{
                    "summary":{
                        "free":{
                            "bytes":301363200000
                        },
                        "available":{
                            "bytes":285240508416
                        },
                        "total":{
                            "bytes":316928753664
                        }
                    }
                },
                "transport":{
                    "rx":{
                        "bytes":463928714616
                    },
                    "tx":{
                        "bytes":268871680483
                    }
                },
                "timestamp":1514183575933,
                "jvm":{
                    "gc":{
                        "collectors":{
                            "old":{
                                "collection":{
                                    "count":46,
                                    "ms":17897
                                }
                            },
                            "young":{
                                "collection":{
                                    "count":45577,
                                    "ms":1566930
                                }
                            }
                        }
                    },
                    "mem":{
                        "pools":{
                            "young":{
                                "max":{
                                    "bytes":907345920
                                },
                                "peak":{
                                    "bytes":907345920
                                },
                                "peak_max":{
                                    "bytes":907345920
                                },
                                "used":{
                                    "bytes":507279736
                                }
                            },
                            "survivor":{
                                "max":{
                                    "bytes":113377280
                                },
                                "peak":{
                                    "bytes":113377280
                                },
                                "peak_max":{
                                    "bytes":113377280
                                },
                                "used":{
                                    "bytes":20464336
                                }
                            },
                            "old":{
                                "used":{
                                    "bytes":2124598296
                                },
                                "max":{
                                    "bytes":7455834112
                                },
                                "peak":{
                                    "bytes":7101687744
                                },
                                "peak_max":{
                                    "bytes":7455834112
                                }
                            }
                        },
                        "heap":{
                            "used":{
                                "bytes":2652342368
                            },
                            "max":{
                                "bytes":8476557312
                            },
                            "percent":31
                        }
                    }
                },
                "indices":{
                    "indexing":{
                        "index":{
                            "current":0,
                            "ms":50588643,
                            "total":255496640,
                            "rate": 3.0003000300030003,
                            "latency": 0,
                        }
                    },
                    "docs":{
                        "deleted":92164,
                        "count":43873236
                    },
                    "store":{
                        "size":{
                            "bytes":15114468374
                        }
                    },
                    "segments":{
                        "count":1852,
                        "memory":{
                            "bytes":70513752
                        }
                    },
                    "fielddata":{
                        "memory":{
                            "bytes":143512
                        }
                    },
                    "search":{
                        "query":{
                            "ms":3854243,
                            "total":949541,
                            "rate": 0,
                            "latency": 0
                            "current":0
                        }
                    }
                },
                "threadpool":{
                    "search":{
                        "rejected":0,
                        "queue":0,
                        "active":0
                    },
                    "index":{
                        "active":0,
                        "rejected":0,
                        "queue":0
                    },
                    "bulk":{
                        "rejected":0,
                        "queue":0,
                        "active":0
                    }
                }
            }
        },
        "cluster":{
            "name":"elasticsearch-dev5.4"
        }
    }
}
```


###3. 指标含义
####3.1 集群指标
| 指标 | 含义 | 类型 | 样例 |
| :---| :--- | :--- | :--- |
| @timestamp | 时间戳 | date | 2017-12-22T10:14:31.670Z |
| elasticsearch.name | 集群名字 | string | elasticsearch-dev5.4 |
| elasticsearch.cluster.stats.status | 集群状态 | string | green |
| elasticsearch.cluster.stats.indices.count | 索引数量 | long | 1021 |
| elasticsearch.cluster.stats.indices.shards.total | 分片数量 | long | 4554 |
| elasticsearch.cluster.stats.indices.docs.count | 文档数量 | long | 199968167 |
| elasticsearch.cluster.stats.indices.segments.count | 段文件数量 | long | 10879 |
| elasticsearch.cluster.stats.nodes.count.total | 节点数量 | long | 5 |
| elasticsearch.cluster.stats.nodes.jvm.mem.heap.used | jvm heap 已用内存 | long | 5 |
| elasticsearch.cluster.stats.nodes.jvm.mem.heap.max | jvm heap 最大内存 | long | 5 |
| elasticsearch.cluster.stats.nodes.fs.total.bytes | 文件系统总物理存储 | long | 1584643768320 |
| elasticsearch.cluster.stats.nodes.fs.free.bytes | 文件系统剩余物理存储  | long | 1420238610432 |
| elasticsearch.cluster.stats.nodes.fs.available.bytes | 文件系统可用物理存储  | long | 1339625152512 |

####3.2 节点指标
| 指标 | 含义 | 类型 | 样例 |
| :---| :--- | :--- | :--- |
| @timestamp | 时间戳 | date | 2017-12-22T10:00:08.123Z |
| elasticsearch.name | 集群名字 | string | elasticsearch-dev5.4 |
| elasticsearch.node.name | 节点名字 | string | node-192.168.31.35 |
| elasticsearch.node.host | 节点主机 | string | 192.168.31.35 |
| elasticsearch.node.stats.indices.store.size.bytes | 索引文件占用物理存储 | long | 14997937164 |
| elasticsearch.node.stats.indices.segments.memory.bytes | 段文件占用内存 | long | 70333597 |
| elasticsearch.node.stats.indices.fielddata.memory.bytes | fielddata占用内存 | long | 143488 |
| elasticsearch.node.stats.indices.docs.count | 文档数量 | long | 43466621 |
| elasticsearch.node.stats.indices.search.query.total | 查询总量 | long | 254638449 |
| elasticsearch.node.stats.indices.search.query.ms | 查询总耗时 | long | 50208905 |
| elasticsearch.node.stats.indices.search.query.current | 当前查询总量 | long | 0 |
| elasticsearch.node.stats.indices.search.query.rate | 查询速率 | float | 3.8 |
| elasticsearch.node.stats.indices.search.query.latency | 查询延时 | float | 0.0005789473684210527 |
| elasticsearch.node.stats.indices.indexing.index.total | 索引总量 | long | 254638449 |
| elasticsearch.node.stats.indices.indexing.index.ms | 索引总耗时 | long | 50208905 |
| elasticsearch.node.stats.indices.indexing.index.current | 当前索引量 | long | 0 |
| elasticsearch.node.stats.indices.indexing.index.rate | 索引速率 | float | 3.8 |
| elasticsearch.node.stats.indices.indexing.index.latency | 索引延时 | float | 0.0005789473684210527 |
| elasticsearch.node.stats.jvm.mem.heap.used.bytes | jvm heap 已用内存 | long | 2820525344 |
| elasticsearch.node.stats.jvm.mem.heap.max.bytes | jvm heap 最大内存 | long | 8476557312 |
| elasticsearch.node.stats.jvm.mem.heap.percent | jvm heap 内存占用百分比 | long | 33 |
| elasticsearch.node.stats.jvm.mem.pools.young.used.bytes | jvm 新生代(young)已用内存 | long | 754729368 |
| elasticsearch.node.stats.jvm.mem.pools.young.max.bytes | jvm 新生代(young)最大内存 | long | 907345920 |
| elasticsearch.node.stats.jvm.mem.pools.young.peak.bytes | jvm 新生代(young)内存峰值 | long | 907345920 |
| elasticsearch.node.stats.jvm.mem.pools.survivor.used.bytes | jvm 幸存区(survivor)已用内存 | long | 754729368 |
| elasticsearch.node.stats.jvm.mem.pools.survivor.max.bytes | jvm 幸存区(survivor)最大内存 | long | 907345920 |
| elasticsearch.node.stats.jvm.mem.pools.survivor.peak.bytes | jvm 幸存区(survivor)内存峰值 | long | 907345920 |
| elasticsearch.node.stats.jvm.mem.pools.old.used.bytes | jvm 老生代(old)已用内存 | long | 754729368 |
| elasticsearch.node.stats.jvm.mem.pools.old.max.bytes | jvm 老生代(old)最大内存 | long | 907345920 |
| elasticsearch.node.stats.jvm.mem.pools.old.peak.bytes | jvm 老生代(old)内存峰值 | long | 907345920 |
| elasticsearch.node.stats.jvm.gc.collectors.old.collection.count | jvm 老生代(old)gc次数 | long | 46 |
| elasticsearch.node.stats.jvm.gc.collectors.old.collection.ms | jvm 老生代(old)gc耗时 | long | 1566930 |
| elasticsearch.node.stats.jvm.gc.collectors.young.collection.count | jvm 新生代(young)gc次数 | long | 45577 |
| elasticsearch.node.stats.jvm.gc.collectors.young.collection.ms | jvm 新生代(young)gc耗时 | long | 1566930 |
| elasticsearch.node.stats.fs.summary.total.bytes | 文件系统总物理存储 | long | 316928753664 |
| elasticsearch.node.stats.fs.summary.free.bytes | 文件系统剩余物理存储 | long | 301458505728 |
| elasticsearch.node.stats.fs.summary.available.bytes | 文件系统可用物理存储 | long | 285335814144 |
| elasticsearch.node.stats.transport.rx.bytes | 网络传输接收总量 | long | 411330229690 |
| elasticsearch.node.stats.transport.tx.bytes |  网络传输发送总量 | long | 259261454030 |