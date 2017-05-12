---
title: nginx学习——安装
date: 2017-05-12 16:14:25
tags: nginx
categories: nginx
---

# 1. 安装计划
* 系统：ubuntu 16.04 LTS X86_64
* 安装nginx版本：nginx-1.12.0 Stable version
* 安装方式：使用apt安装

# 2. 安装
## 2.1. 查找Ubuntu发行版的`codename`
| Version | Codename | Supported Platforms |
|-------------|-------------|-----|
| 12.04 | precise | x86_64, i386 |
| 14.04 | trusty | x86_64, i386, aarch64/arm64 |
| **16.04** | **`xenial`** | **x86_64, i386, ppc64el, aarch64/arm64** |
| 16.10 | yakkety | x86_64, i386 |
## 2.2. 添加apt sources
将以下内容追加到文件`/etc/apt/sources.list`末尾： 
```shell
deb http://nginx.org/packages/ubuntu/ xenial nginx
deb-src http://nginx.org/packages/ubuntu/ xenial nginx
```
## 2.3. 下载并导入**nginx signing key**
RPM软件包和Debian / Ubuntu存储库都使用数字签名来验证下载包的完整性和起源。为了检查签名，需要下载 [nginx签名密钥](http://nginx.org/keys/nginx_signing.key) 并将其导入rpm或apt 程序的密钥环：
```shell
sudo apt-key add /%密钥所在文件夹%/nginx_signing.key
```
## 2.4. 执行安装
```shell
sudo apt-get update
sudo apt-get install nginx
```
## 2.5. 安装完成
```shell
$ nginx -h
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

# 3. 参考文档
> nginx documentation：[Installing nginx](http://nginx.org/en/linux_packages.html)