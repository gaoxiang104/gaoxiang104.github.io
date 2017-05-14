---
title: nginx学习(2)——进程和运行时控制
date: 2017-05-13 13:38:57
tags: nginx
categories: nginx
---

# 1. 主进程和工作进程（Master and Worker Processes）

>  NGINX has one master process and one or more worker processes. If caching is enabled, the cache loader and cache manager processes also run at startup.

NGINX有一个主进程和一个或多个工作进程。如果启用缓存，缓存加载程序和缓存管理器进程也将在启动时运行。

> The main purpose of the master process is to read and evaluate configuration files, as well as maintain the worker processes.

主程序的主要目的是读取和评估配置文件以及维护工作进程。

> The worker processes do the actual processing of requests. NGINX relies on OS-dependent mechanisms to efficiently distribute requests among worker processes. The number of worker processes is defined in the nginx.conf configuration file and can be fixed for a given configuration or automatically adjusted to the number of available CPU cores (see [worker_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)).

工作进程处理真实的请求。NGINX依赖于依赖于操作系统的机制来有效地在工作进程之间分配请求。工作进程的数量在nginx.conf配置文件中定义，并且可以针对给定的配置进行修复，或者自动调整为可用CPU内核的数量（参见 [worker_processes](http://nginx.org/en/docs/ngx_core_module.html#worker_processes)）。


# 2. nginx控制
首先查看`nginx`命令的`help`
```shell
# nginx -h 
nginx version: nginx/1.12.0
Usage: nginx [-?hvVtTq] [-s signal] [-c filename] [-p prefix] [-g directives]

Options:
  -?,-h         : this help
  -v            : show version and exit
  -V            : show version and configure options then exit
  -t            : test configuration and exit
  -T            : test configuration, dump it and exit
  -q            : suppress non-error messages during configuration testing
  -s signal     : send signal to a master process: stop, quit, reopen, reload
  -p prefix     : set prefix path (default: /etc/nginx/)
  -c filename   : set configuration file (default: /etc/nginx/nginx.conf)
  -g directives : set global directives out of configuration file
```

## 2.1. 启动nginx
启动方式如下：
```shell
# nginx -c /etc/nginx/nginx.conf
```
*备注：网上找的启动方式，看help并不明白 nginx -c 就是start*

## 2.2. nginx -s
通过`nginx -s signal`来控制主进程，下面是`signal`的详细解释：
* stop : 快速关闭服务
* quit : 正常关闭服务,该命令应该在启动nginx的同一用户下执行。
* reload : 重新加载配置文件
* reopen : 重新打开日志文件

> Changes made in the configuration file will not be applied until the command to reload configuration is sent to nginx or it is restarted. To reload configuration, execute:

nginx重启之前，配置文件中修改的内容将不会被应用，如果需要重新加载配置文件，需要执行：
```shell
nginx -s reload
```

> Once the master process receives the signal to reload configuration, it checks the syntax validity of the new configuration file and tries to apply the configuration provided in it. If this is a success, the master process starts new worker processes and sends messages to old worker processes, requesting them to shut down. Otherwise, the master process rolls back the changes and continues to work with the old configuration. Old worker processes, receiving a command to shut down, stop accepting new connections and continue to service current requests until all such requests are serviced. After that, the old worker processes exit.

一旦主进程收到要重新加载配置的命令，它将检查新配置文件的语法有效性，并尝试应用其中提供的配置。如果这是成功的，主进程将启动新的工作进程，并向旧的工作进程发送消息，请求它们关闭。否则，主进程回滚更改，并继续使用旧配置。老工作进程，接收关闭命令，停止接受新连接，并继续维护当前请求，直到所有这些请求得到维护。之后，旧的工作进程退出。

## 2.3. 查看nginx进程列表
获取正在运行的nginx进程列表，可以使用下面的命令：
```shell
# ps -ax | grep nginx
 9345 ?        Ss     0:00 nginx: master process nginx -c /etc/nginx/nginx.conf
 9358 ?        S      0:00 nginx: worker process
11734 pts/18   S+     0:00 grep --color=auto nginx
```

可以使用`kill nginx master process`正常关闭nginx :
```shell
keill -s QUIT 9345
```

# 3. 配置文件结构介绍
> The way nginx and its modules work is determined in the configuration file. By default, the configuration file is named `nginx.conf` and placed in the directory `/usr/local/nginx/conf`, `/etc/nginx`, or `/usr/local/etc/nginx`.

配置文件中确定nginx及其模块的工作方式。默认的配置文件是 `/etc/nginx/` 目录中的 `nginx.conf` 文件。

# 4. 参考文献
> nginx documentation：[Beginner’s Guide](http://nginx.org/en/docs/beginners_guide.html)
> [Runtime control : ](https://www.nginx.com/resources/admin-guide/processes-and-runtime-control/)