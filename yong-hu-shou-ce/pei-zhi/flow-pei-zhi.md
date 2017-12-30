# flow配置

本章介绍flow采集的详细配置参数。一般来说，除非需要对采集做调优配置，否则无需试图通过界面或者手工的方式修改这些配置。不过，对于内部功能性测试有必要了解这些细节，并设计适当的测试用例。

flow采用`Apache风格`的配置文件。一个完整的flow配置文件，通常包含四种模块`Input`，`Output`，`Extension`,`Route`。例如：

```
<Extension charconv>
  Module xm_charconv
</Extension>


<Input input_15E>
  Module im_file
  SavePos TRUE
  ReadFromLast FALSE
  Recursive FALSE
  RenameCheck TRUE
  ActiveFiles 10
  File "/tmp/test/*.log"
  Exec convert_fields("gbk", "utf-8");$@@topic="paichutest";$@@streamingid="734";$@@reqid="3ec";
</Input>


<Output out>
  Module om_lumberjack
Host 172.16.128.34
Port 15044
  Batch 300
  Version 2
  UseCompress FALSE
  UseSSL FALSE
</Output>

<Route r>
  Path input_15E  => out
</Route>
```

## 全局配置

这里罗列一些常用的全局配置参数

| 名称       | 是否必填   | 默认值  | 说明                                       |
| :------- | :----- | :--- | :--------------------------------------- |
| LogLevel | Option | INFO | 日志级别CRITICAL, ERROR, WARNING, INFO, DEBUG |


## 模块类型

| 名称            | Linux | Windows | AIX  | 说明                        |
| :------------ | :---: | :-----: | :--: | :------------------------ |
| im_archive    |  ✔️   |    ✘    |  ✘   | 归档压缩文件采集模块                |
| im_mseventlog |   ✘   |   ✔️    |  ✘   | windows 2003版本的winlog采集模块 |
| im_msvistalog |   ✘   |   ✔️    |  ✘   | windows 2008版本的winlog采集模块 |
| im_file       |  ✔️   |   ✔️    |  ✔️  | 文件采集模块                    |
| im_tcp        |  ✔️   |   ✔️    |  ✔️  | tcp采集模块                   |
| im_udp        |  ✔️   |   ✔️    |  ✔️  | udp采集模块                   |
| om_file       |  ✔️   |   ✔️    |  ✔️  | 输出到文件模块                   |
| om_lumberjack |  ✔️   |   ✔️    |  ✔️  | 输出到路由的模块                  |
| om_tcp        |  ✔️   |   ✔️    |  ✔️  | 输出到tcp的模块                 |
| om_http       |  ✔️   |   ✔️    |  ✔️  | 输出到http的模块                |

## 模块配置

一个模块实例用一个“类似”`xml`的方式配置：

```
<Input instancename>
  Module im_file
  ...
</Input>
```

- instancename: 模块实例名，这个实例名需要在`Route`中引用
- Module: 模块类型，可填写，flow支持的模块类型。参见`模块类型`


### im_file


| 名称               | 是否必填    | 默认值   | 说明                                       |
| :--------------- | :------ | :---- | :--------------------------------------- |
| File             | Require |       | 文件名或含有通配符的文件路径                           |
| BlackList        | Option  |       | 黑名单过滤列表，逗号间隔。如"\*/1/11/\*","\*/1/Memorizethepassedtime.log","\*/2/21/\*.txt","\*/2/mood.txt" |
| WhiteList        | Option  |       | 白名单过滤列表，类似BlackList                      |
| SavePos          | Option  | TRUE  | 是否保存采集点位置                                |
| Recursive        | Option  | TRUE  | 是否递归遍历File指定的目录                          |
| RenameCheck      | Option  | FALSE | 是否开启，重命名检测                               |
| ReadFromLast     | Option  | TRUE  | 首次采集时，是否只从最后开始采集                         |
| PollInterval     | Option  | 1     | 对已经打开的文件，check是否有新数据的时间间隔，默认1s           |
| DirCheckInterval | Option  | 2     | 检查目录中的新文件和文件变化的周期，默认2s                   |
| ActiveFiles      | Option  | 10    | 同时打开的文件数量                                |
| Enable           | Option  | TRUE  | 是否启用                                     |


### om_lumberjack



