# Agent安装

## 版本约定

Agent的各个不同平台的版本遵循如下命名约定

| 平台 | 包文件名说明 | 示例 |
| :--- | :--- | :--- |
| Linux | mave-{版本}-{适用产品或项目}-{日期}-Linux.{arch}.GLIBC\_{glibc-version}.tar.gz | mave-5.0.0-sharplook-171128.Linux.x86\_64.GLIBC\_2.10.tar.gz |
| AIX | mave-{版本}-{适用产品或项目}-{日期}-AIX6.1.tar.gz | mave-5.0.0-sharplook-171128.AIX6.1.tar.gz |
| Windows | mave-{版本}-{适用产品或项目}-{日期}.windows{版本}.{arch}.tar.gz | mave-5.0.0-sharplook-171128.windows03.x86\_64.tar.gz |

首先，需要根据不同的平台选择合适软件包。

> 版本：版本需要配合server的版本号，否则可能会有兼容性问题。windows版本只有两种windows03和windows08，对应windows server 2003和高于windows server 2008，详见下面对windows版本的安装说明。
>
> 适用产品或项目：表示这个build的agent用于那个特定的产品或项目，比如sharplook表示夏洛克

## Windows

### 准备

windows系统由于event log的api接口不兼容，需要把windows平台细分为两大类

* `windows 2003 server`和`windows xp`

* `windows 2008 server`以上和`windows vista`以上

针对两大类，需正确选择合适的发布包。

将Agent安装到服务管理器中，你需要管理员权限。

### 安装

1. 下载合适的安装包，并解压到合适的目录，假设是`C:\Program Files`
2. 用管理员身份打开`cmd.exe`，进入到`C:\Program Files\mave`
3. 运行命令`bin\mave -i`安装和注册服务，如果正确安装，会显示`Service successfully installed`，并且可以在Service Manager中看到服务`EOITek Mave(Agent Daemon)`
4. 你可以注意一下输出的mave版本信息，这有助于将来报告问题

### 卸载

1. 用管理员身份打开`cmd.exe`，进入到`C:\Program Files\mave`
2. 运行命令`bin\mave -u`，如果卸载成功，会显示`Service successfully uninstalled`
3. 删除`C:\Program Files\mave`

## Unix

### 准备

* Linux平台下，需要确认`glibc`的版本来选择合适的安装包。目前的发布中，支持最低`2.10`版本（对应`RHEL 6.8`）。一般目前的发行版都应该大于这个版本。
* AIX平台，目前在AIX 6.1下测试，不保证支持低于AIX 6.1。

根据Unix的平台选择合适的发布包。

### 安装

1. 下载合适的安装包，并解压到合适的目录，假设是`/itoa/app/mave`
2. 如果要将agent注册到系统开机自启动，可运行`/itoa/app/mave/bin/service-unix.sh`

### 卸载

1. 直接删除解压的目录即可

# Server安装

# Hub安装



