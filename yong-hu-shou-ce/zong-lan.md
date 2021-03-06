数据采集模块是夏洛克智能运维分析平台的数据收集系统，负责通过各种方式收集和传输业务数据。理论上业务数据的类型是无限的，例如：监控性能数据、监控告警数据、系统日志、应用程序日志。为了在复杂的IT环境下保持较好的环境适应性，数据采集模块设计两种采集子模块：

- 分布式采集
- 集中式采集

无论是哪种采集模块，最终的数据输出目标是确定的。数据采集模块将最终的数据输出到Kafka集群中，以便数据处理模块处理。Kafka集群在这个数据传输过程中扮演了数据缓冲的作用，即当生产和消费速度不对等时，Kafka可以缓冲数据，并有效的隔离这种速度差异。

# 分布式采集

分布式采集模块的设计方法是，将Agent安装到目标主机上（物理机和虚机机都可以），采集主机的性能、日志等业务数据，并通过某种通路上报到服务端。这种方式适用于如下场景：

1. 数据实时性要求较高的场合。
2. 只有通过代理才能采集到数据的场合。

## 组件和架构

分布式采集模块分为`代理程序`和`服务程序`两大组件。代理程序（又称为agent）需被安装部署在目标主机上，负责具体的采值任务。服务程序（称为server）安装在多台服务器主机上，负责与代理程序通信，对代理程序进行控制，以及接收代理程序采集到的数据。服务程序随后将数据转发到Kafka集群。在发布包中，通常代理程序名叫`mave-xxx.tar.gz`，服务程序名叫`server-xxx.tar.gz`。


### 代理程序

代理程序的采值能力应尽量丰富，但丰富的功能必然导致代理程序越来越臃肿。为了平衡这个问题，代理程序将功能模块进一步物理划分为管理程序和采值程序。根据具体的需要，可将管理程序和不同的采值程序组合起来使用，最大限度降低代理程序的复杂度，降低系统资源消耗以及保持轻便。管理程序存在的意义在于，将服务端的配置信息下发给具体的采值程序，这样对于采值程序来说只需专注在实现功能上，无需关注配置信息如何更改。

管理程序是一个文件名叫`mave`的可执行文件，启动整个代理程序的过程就是启动`mave`的过程，`mave`会自动分别启动需要采值程序。

采值程序丰富多彩，各有各的功能，将来也会新增和升级。目前已发布的采值程序有如下几种：

- `flow`：一个流采集器，可采集本地文件和目录、归档文件、监听并采集tcp/udp流、windows事件日志。基于这些采集的流，可进行按行分割、多行合并、syslog解析。
- `upbeat`：性能采集器，适用于采集主机性能指标，以及一些常见软件的性能指标采集。
- `cronext`: 自定义脚本采集器。相当于一个加强版crontab。

这些采值程序的平台兼容性如下：

| 名称      | Linux | Windows | AIX  | 说明                                       |
| :------ | :---: | :-----: | :--: | :--------------------------------------- |
| flow    |  ✔️   |   ✔️    |  ✔️  |                                          |
| upbeat  |  ✔️   |   ✔️    |  ✘   | widows平台目前理论支持，但没有经过测试，也没有发布在windows版本中。AIX暂没有支持计划 |
| cronext |  ✔️   |    ✘    |  ✔️  |                                          |

一个完整的代理程序包，在安装和运行后典型的目录结构如下：

```
/itoa/app/mave
├── bin
│   ├── downloader
│   ├── init
│   ├── mave
│   ├── send_udp_msg
│   ├── service-unix.sh
│   ├── start
│   ├── stop
│   ├── upgrade
│   └── VERSION
├── cache
│   └── configuration.dat
├── conf
│   ├── mave.conf
│   └── mave.conf.last
├── enrollscript.1513932923.ms
├── logs
│   └── mave.log
├── mainifest
├── probes
│   ├── bin
│   │   ├── cronext
│   │   │   ├── cronext
│   │   │   └── VERSION
│   │   ├── flow
│   │   │   ├── flow
│   │   │   └── VERSION
│   │   └── upbeat
│   │       ├── upbeat
│   │       └── VERSION
│   └── cache
│       └── flow
│           └── data
└── run
    └── mave.pid
```

在这里重点关注，如下几个目录和文件：

- bin/mave: 这是mave的主程序

- bin/VERSION: 这是mave的版本文件，其中的内容是mave的版本信息

- conf/mave.conf: 这是mave的配置文件

- cache/configuration.dat: 这是mave的缓存文件，其中缓存了当前运行的mave实例的唯一Id，该Id由服务端程序生成和下发

- probes/bin: 这个目录存放采值程序，每个采值程序对应一个额目录，目录中一般是采值程序的可执行文件和版本文件


### 服务程序

服务程序主要承担两方面职责：1. 对代理程序进行控制和配置，2. 接收代理程序上报的采值数据并转发。服务程序又分为两个独立的组件来完成这两项任务:

- `cell`: 负责对代理程序进行控制。提供夏洛克其他组件的调用接口，以实现对agent的控制和配置下发。
- `router`: 负责接收代理程序上好的采值数据并转发到kafka。

当前版本的`cell`和`router`需要用户自行分别启动。



# 集中式采集

