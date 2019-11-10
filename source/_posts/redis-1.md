---
title: redis学习(1)——安装
date: 2017-06-02 14:18:32
tags: redis
categories: redis
---

# 一.安装

系统：ubuntu16.04;

redis版本：Redis 3.2.9 is the latest stable version;

## 1.1.下载redis安装包
到redis官网[https://redis.io](https://redis.io)查看当前稳定版本，使用`wget`下载：
```linux
# cd /use/local/src/
# wget http://download.redis.io/releases/redis-3.2.9.tar.gz
```

<!-- more -->

## 1.2.解压
```linux
# tar -zxvf redis-3.2.9.tar.gz 
redis-3.2.9/
redis-3.2.9/.gitignore
redis-3.2.9/00-RELEASENOTES
redis-3.2.9/BUGS
redis-3.2.9/CONTRIBUTING
redis-3.2.9/COPYING
redis-3.2.9/INSTALL
redis-3.2.9/MANIFESTO
redis-3.2.9/Makefile
redis-3.2.9/README.md
redis-3.2.9/deps/
...
```

## 1.3.编译
```linux
# cd redis-3.2.9/
# ls
00-RELEASENOTES  CONTRIBUTING  deps     Makefile   README.md   runtest          runtest-sentinel  src    utils
BUGS             COPYING       INSTALL  MANIFESTO  redis.conf  runtest-cluster  sentinel.conf     tests
# make 
```

如果提示以下错误信息：
```linux
/bin/sh: cc: 未找到命令
```
说明**未安装gcc**，需安装`gcc`后，再进行`make`。

`make`成功后，src目录下多出一下文件，如下：
```linux
# ll -t
总用量 18320
-rwxr-xr-x 1 root root   25213 6月   2 14:59 redis-check-aof
-rw-r--r-- 1 root root   33696 6月   2 14:59 redis-check-aof.o
-rwxr-xr-x 1 root root 3092014 6月   2 14:59 redis-check-rdb
-rwxr-xr-x 1 root root  337927 6月   2 14:59 redis-benchmark
-rw-r--r-- 1 root root  107744 6月   2 14:59 redis-benchmark.o
-rwxr-xr-x 1 root root  491135 6月   2 14:59 redis-cli
-rw-r--r-- 1 root root  366208 6月   2 14:59 redis-cli.o
-rwxr-xr-x 1 root root 3092014 6月   2 14:59 redis-sentinel
-rwxr-xr-x 1 root root 3092014 6月   2 14:59 redis-server
```

## 1.4.安装 
执行make install：
```linux
# make install

Hint: It's a good idea to run 'make test' ;)

    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install
    INSTALL install

```
可以查看`/usr/local/bin`下已有这些文件：
```linux
# cd /usr/local/bin/
# ll redis*
-rwxr-xr-x 1 root root  337927 6月   2 15:11 redis-benchmark
-rwxr-xr-x 1 root root   25213 6月   2 15:11 redis-check-aof
-rwxr-xr-x 1 root root 3092014 6月   2 15:11 redis-check-rdb
-rwxr-xr-x 1 root root  491135 6月   2 15:11 redis-cli
lrwxrwxrwx 1 root root      12 6月   2 15:11 redis-sentinel -> redis-server
-rwxr-xr-x 1 root root 3092014 6月   2 15:11 redis-server
```
查看redis版本：
```linux
# redis-server -v
Redis server v=3.2.9 sha=00000000:0 malloc=libc bits=64 build=74d1b0779b412792
```
能显示以上信息，说明redis安装完成。

# 二.修改配置

## 2.1.创建配置文件目录、dump file目录、进程pid目录、log目录

配置文件放在`/etc/`下，创建redis目录：
```linux
# cd /etc/
# mkdir redis
```

dump file、进程pid、log等，一般放在`\var\`目录下：
```linux
# mkdir redis
# cd redis/
# mkdir data log run
# ls
data  log  run
```
至此，目录创建完毕

## 2.2.copy配置文件，修改配置参数

首先copy源码包下的`redis.conf`文件至`/etc/redis/`目录
```linux
# cd /etc/redis/
# cp /usr/local/src/redis-3.2.9/redis.conf .
# ll
总用量 48
-rw-r--r-- 1 root root 46695 6月   2 15:45 redis.conf
```

修改`redis.conf`中的配置参数：

修改pid目录为`/var/redis/run/`
```shell
# If a pid file is specified, Redis writes it where specified at startup
# and removes it at exit.
#
# When the server runs non daemonized, no pid file is created if none is
# specified in the configuration. When the server is daemonized, the pid file
# is used even if not specified, defaulting to "/var/run/redis.pid".
#
# Creating a pid file is best effort: if Redis is not able to create it
# nothing bad happens, the server will start and run normally.
pidfile /var/redis/run/redis_6379.pid
```

修改dump目录为`/var/redis/data/`
```shell
# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
dir /var/redis/data
```

修改log存放目录为`/var/redis/log/`
```shell
# Specify the log file name. Also the empty string can be used to force
# Redis to log on the standard output. Note that if you use standard
# output for logging but daemonize, logs will be sent to /dev/null
logfile /var/redis/log/redis.log
```

修改daemonize为yes，即默认以后台程序方式运行
```shell
# By default Redis does not run as a daemon. Use 'yes' if you need it.
# Note that Redis will write a pid file in /var/run/redis.pid when daemonized.
daemonize yes
```
至此redis的简单配置完成，下面启动redis服务

# 三.启动脚本配置
## 3.1.创建redis启动脚本
copy源码包下的`utils/redis_init_script`至`/etc/init.d/`目录下，并改名为`redis`:
```linux
# cd /etc/init.d/
# cp /usr/local/src/redis-3.2.9/utils/redis_init_script .
# mv redis_init_script redis
```
修改启动文件`redis`：
```shell
REDISPORT=6379
EXEC=/usr/local/bin/redis-server
CLIEXEC=/usr/local/bin/redis-cli

PIDFILE=/var/run/redis_${REDISPORT}.pid
CONF="/etc/redis/redis.conf"
```

## 3.2.启动/关闭redis服务
启动：
```linux
# /etc/init.d/redis start
Starting Redis server...
[root@VM_11_46_centos init.d]# ps -ax | grep redis
 2979 pts/0    S+     0:00 vim redis.conf
 3123 ?        Ssl    0:00 /usr/local/bin/redis-server 127.0.0.1:6379
 3127 pts/2    S+     0:00 grep --color=auto redis
```

关闭：
```linxu
# /etc/init.d/redis stop
```

# 四.参考文献

[http://blog.csdn.net/ludonqin/article/details/47211109](http://blog.csdn.net/ludonqin/article/details/47211109)