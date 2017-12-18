kafka模块用于监控kafka的指标，
从这些指标中可以了解kafka的当前运行状态。
主要包括生产tps，消费tps，topic的offset信息等。


###配置文件
```
- module: kafka
  metricsets: ["partition","consumergroup"]
  period: 10s
  hosts: ["localhost:9092"]
  #topics: []
  #groups: []
```
- `module` 采集模块为kafka
- `metricsets` 监控项，partition表示分区相关（生产者角度），consumergroup消费相关（消费者角度）
- `period` 采集间隔时间
- `hosts` 被采集的服务器列表
- `topics` topic过滤数组，如果被设置，那么直采集数组内的topic；如果未设置，则采集所有topic
- `groups` groups过滤数组，如果被设置，那么直采集数组内的group；如果未设置，则采集所有group


###指标数据样例
####生产者角度
```
{
    "@timestamp":"2017-12-18T07:49:00.920Z",
    "kafka":{
        "partition":{
            "produce":{
                "tps":9.596161535385846
            },
            "topic":{
                "name":"wangdi-mb-test"
            },
            "broker":{
                "id":0,
                "address":"localhost:9092"
            },
            "partition":{
                "id":0,
                "leader":0
            },
            "offset":{
                "newest":108,
                "oldest":0,
                "range":108
            }
        }
}
```
####消费者角度
```
{
    "@timestamp":"2017-12-18T07:49:00.920Z",
    "kafka":{
        "consumergroup":{
            "partition":0,
            "consume":{
                "tps":6.598680263947211
            },
            "meta":"",
            "client":{
                "host":"consumer-1",
                "member_id":"172.16.128.198",
                "id":"consumer-1"
            },
            "topic":"wangdi-mb-test",
            "broker":{
                "id":0,
                "address":"172.16.128.198:9092"
            },
            "id":"wangdi_test_client",
            "offset":66
        }
    }
}
```


###指标含义
####生产者角度
| 指标 | 含义 |
| :---| :--- |
| @timestamp | 时间戳 |
| kafka.partition.topic.name | topic名字 |
| kafka.partition.produce.tps | 生产tps |
| kafka.partition.offset.newest | partition的最新offset |
| kafka.partition.offset.oldest | partition的最旧offset |
| kafka.partition.offset.range | newest-oldest |
| kafka.partition.partition.id | partition的id |
| kafka.partition.partition.leader | partition的leader broker id |
| kafka.partition.broker.id | broker的id |
| kafka.partition.broker.address | broker的hostname |
####消费者角度
| 指标 | 含义 |
| :---| :--- |
| @timestamp | 时间戳 |
| kafka.consumergroup.partition | 消费组的partitionid |
| kafka.consumergroup.topic | 消费组的topic |
| kafka.consumergroup.id | 消费组的id |
| kafka.consumergroup.meta | 消费组的meta |
| kafka.consumergroup.offset | 已消费到的offset |
| kafka.consumergroup.consume.tps | 消费tps |
| kafka.consumergroup.client.host | kafka client的host |
| kafka.consumergroup.client.member_id | kafka client的消费组成员id |
| kafka.consumergroup.client.id | kafka client的id |
| kafka.consumergroup.broker.id | broker的id |
| kafka.consumergroup.broker.address | broker的hostname |

