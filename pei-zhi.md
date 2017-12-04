# Agent配置

> $MAVE\_HOME表示agent被安装到的路径，比如/itoa/app/mave

## 配置mave

mave的配置文件放在$MAVE\_HOME/conf目录下，mave能识别三个配置文件，分别是`default.conf`, `mave.conf`, `local.conf`，mave最终的工作行为由这三个配置文件共同生效。在出现配置冲突的时候，`local.conf`的优先级最高，即local.conf中的配置会覆盖mave.conf和default.conf。`mave.conf`的配置优先级仅次于local.conf，mave.conf通常不需要编辑，因为其中的内容是由服务端下发生成的（首次安装时需要配置一次server地址）。`default.conf`的优先级最低。

mave的配置文件分为三个部分`[core]`配置节、`[general]`配置节和`命名`配置节。下面是一个样例：

```
[core]
loglevel = DEBUG2

[general]
cmd_channels = mac_go
watch_procs = ping

[mac_go]
host = 192.168.31.210
port = 8899
main = Y

[ping]
cmd = /bin/ping
args = 192.168.31.23
conf = /etc/eoic.conf
upstream = mac_go
cpu = 0.05
```

### core

`core`节用于配置程序运行的基本参数：

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| log\_level | Option | INFO | 日志级别。在守护进程模式下将输出在$MAVE\_HOME/logs目录下 |
| log\_count | Option | 5 | 日志最大个数。为了避免日志爆盘，日志文件个数可通过这个值限制，与之相对应的还有log\_size配置 |
| log\_size | Option | 10\*1024\*1024 | 每个日志文件的最大大小。参见log\_count |
| ring\_buf | Option | 4096 | 缓存常驻进程输出的缓冲区大小，单位字节。由于常驻进程的输出会不断产生，所以无法使用线性缓冲区，使用一个循环队列可以只保留最近的输出 |
| default\_proc\_timeout | Option | 5000 | 在进程管理时，对进程设置的超时时间，单位ms。为了防止脚本或命令永远驻留执行，使用这个默认超时时间。不过在接口层面，服务端依然可以设置对进程退出的希望超时时间 |
| proc\_autorestart\_interval | Option | 60000 | 常驻进程重启时间。当检测到常驻进程异常退出时，经过该参数指定的秒数重启进程 |
| cgroups\_enable | Option | 0 | （Linux）是否开启cgroups来对进程进行资源限制。这是个全局开关 |
| cgroups\_hierarchy | Option | /cgroup | （Linux）当系统支持cgroups时，mave会尝试用现有的cgroups文件系统来实现cgroups功能。然而，有时系统没有默认挂载cgroups文件系统，此时需要自动挂载，这个配置项告诉mave将cgroups文件系统挂载在什么目录下 |
| protocol\_retry\_interval | Option | 30000 | 与服务端内部通信时，如果失败，间隔多久重试 |
| protocol\_timeout | Option | 10000 | 与服务端内部通信时，等待服务端响应的超时时间 |
| heartbeat\_interval | Option | 60000 | 心跳周期 |
| udp\_port | Option | 8190 | mave监听本地的一个udp端口来接收runtime control。详见故障诊断 |

### general

`general`节用于配置服务器和进程

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| cmd\_channels | Require |  | 该参数指定一个网络会话的命名配置。比如如果配置有一个叫cell的网络会话。这里应配置为：cmd\_channels = cell |
| watch\_procs | Option | None | 配置若干个进程会话的命名配置。详见进程会话 |

### 命名配置

命名配置是指一些名称不确定的配置节，这些配置节分为两类：`网络会话`和`进程会话`。这些名称虽然是不确定的，但是都应当在`general`中有所体现。

#### 网络会话

只能配置一个有效的网络会话，这个网络会话必须用于`general`中的`cmd_channels`，否则没有意义。 为了让mave正常运行，有且只能由一个网络会话。需要注意的是，`mave.conf`中的网络会话配置项，可能会在于服务器通信过程中被修改，所以一般来说只需要第一次配置成功，以后无需在意其改动。

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| \[name\] | Require | None | 配置节名称，这个名称用于cmd\_channels |
| host | Require | None | 服务端主机列表，用逗号分割的主机名或ip地址。对应服务端的多台cell |
| port | Require | None | 服务端主机所使用的端口，目前仅能配置一个端口。意味着服务端的cell集群的任意节点只能用相同的端口 |
| main | Require | None | 1或0，表示这个网络会话是否是主会话，此处必须填1。此项将来可能弃用 |

#### 进程会话

> 除非你希望自主在default.conf或local.conf中配置不受下发影响的常驻进程，否则一般来说永远不用修改`mave.conf`的进程会话。

进程会话用于配置常驻进程，通常这是由服务端下发的。配置的进程会话，需要配置到`watch_procs`才有意义。

`watch_procs`中配置多个进程会话只要用逗号分隔多个`[name]`即可。

| 名称 | 是否必填 | 默认值 | 说明 |
| :--- | :--- | :--- | :--- |
| \[name\] | Require | None | 配置节名称，这个名称用于watch\_procs |
| cmd | Require | None | 启动进程的可执行文件路径，这里可以基于$MAVE\_HOME配置相对路劲 |
| args | Option | None | 进程执行的命令行参数，空格分隔多个参数，就与普通的unix进程命令行参数写法相同 |
| cwd | Option | None | 进程执行的工作目录 |
| conf | Option | None | 进程配置文件所在路径，可以基于$MAVE\_HOME配置相对路径 |
| upstream | Require | None | 指向网络会话的名称，此项将来可能弃用 |
| cpu | Option | None | （Linux）希望控制进程的cpu占用率的配置，只在linux的cgroups\_enable的情况下才生效。浮点值，比如0.05表示5% |
| memory | Option | None | （Linux）希望控制进程的memory占用率的配置，只在linux的cgroups \_enable的情况下才生效。单位MB。 |

# Server配置

## 配置cell

## 配置router

# Hub配置

## 配置hub

## 配置KC



