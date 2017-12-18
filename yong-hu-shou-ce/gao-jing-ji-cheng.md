# 概述

实现将第三方监控系统的告警事件集成到夏洛克平台，便于统一的事件管理、展现、通知等。支持如下几种接入方式：

* mdispatcher工具

# mdispatcher

mdispatcher是一个跨平台的事件发送工具，专门用于将事件（如监控告警）收集至夏洛克平台。mdispatcher支持`tcp`和`udp`两种协议，在不同的IT环境下，可根据命令行参数选择不同的协议。两种协议主要有如下优缺点：

| 模式 | 优点 | 缺点 |
| :--- | :--- | :--- |
| TCP方式 | 可靠的网络传输，在复杂网络环境下，可保证数据必达 | 基于差错控制的TCP协议，可能会堵塞数据流，从而会产生mdispatcher进程本身产生常驻实例，并占用一些磁盘空间来临时存放未发送成功的告警数据 |
| UDP方式 | 由于数据无需保证必达，不会造成实例积压，对原系统影响最小 | 在网络环境不稳定的情况下，数据无法保证必达 |

## 命令行参数说明

```bash
mdispatcher -n 192.168.47.38:1828 
-a ALERT 
-b region=xx;appname=xx;hostname=xx;ipaddress=xx;object=;instance=xxx;parameter=xxx;severity=xxx;msg=xxx;paramvalue=xxx;threshold=xxx;time=xxx
```

* -n: 接收服务器的IP地址和端口
* -p: \[tcp\|udp\]协议选择，目前只支持udp
* -a: 事件类型。对于告警填写`ALERT`
* -c: 压缩选项，如果指定则会对包进行压缩
* -b: 具体的事件内容。针对`ALERT`类型的事件，预定的参数如下（这些参数都不是必填的）：
  * region: 区域标识，比如机房信息
  * appname: 应用系统名
  * hostname: 产生告警的主机名
  * ipaddress: 产生告警的主机IP
  * object: 告警类型，如CPU
  * instance: 告警实例，如hdisk0
  * parameter: 告警参数
  * severity: 告警级别
  * msg: 告警内容
  * value: 当前值
  * threshold: 告警阈值
  * time: 告警产生时间



