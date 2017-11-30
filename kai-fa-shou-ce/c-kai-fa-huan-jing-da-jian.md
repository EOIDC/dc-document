# 说明

本章主要说明如何搭建 c 程序的开发环境，如 mave，flow，cronext 等。

在编译这些 c 程序之前需要先安装 apr，libarchive，pcre，zlib，openssl 等库。

其中 apr，libarchive，pcre 是要编译安装的，而 zlib，openssl 只编译不安装。

# 安装 apr

首先从 192.168.31.142:/root/apr-1.6.2.tar.gz 获取源码包。

tar zxvf apr-1.6.2.tar.gz

cd  apr-1.6.2

./configure --disable-dso --prefix=/usr

make

make install

# 安装 libarchive

首先从 192.168.31.142:/root/libarchive-3.2.1.tar.gz 获取源码包。

tar zxvf libarchive-3.2.1.tar.gz

cd  libarchive-3.2.1

./configure --prefix=/usr

make

make install

# 安装 pcre

首先从 192.168.31.142:/root/pcre-8.40.tar.gz 获取源码包。

tar zxvf pcre-8.40.tar.gz

cd  pcre-8.40

./configure --disable-cpp --prefix=/usr

make

make install

# 安装 zlib

首先从 192.168.31.142:/root/zlib-1.2.11.tar.gz 获取源码包。

tar zxvf zlib-1.2.11.tar.gz     //建议解压到 /root/zlib-1.2.11 目录，因为 flow 默认会先到这个目录下寻找 libz 库文件

cd  zlib-1.2.11

./configure

make

# 安装 openssl

首先从 192.168.31.142:/root/openssl-1.0.2l.tar.gz 获取源码包。

tar zxvf openssl-1.0.2l.tar.gz    //建议解压到 /root/openssl-1.0.2l 目录，flow 默认会先到这个目录下寻找 libcrypto, libssl 库文件

cd  openssl-1.0.2l

./config

make

