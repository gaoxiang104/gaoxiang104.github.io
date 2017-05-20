---
title: nginx学习(3)——配置文件管理
date: 2017-05-20 22:45:47
tags: nginx
categories: nginx
---

# 1. 配置文件在哪？
> NGINX Plus is similar to other services in that it has a text-based configuration file written in a particular format. By default the file is named `nginx.conf` and placed in the `/etc/nginx` directory.

NGINX Plus与其他服务类似，因为它具有以特定格式编写的基于文本的配置文件。默认情况下，文件名为`nginx.conf`并放在`/etc/nginx`目录中。(对于开源NGINX产品，所述位置取决于用于安装NGINX和操作系统程序包的系统。我的系统是`ubuntu`，`nginx.conf`所在目录是：`/etc/nginx`)

# 2. 配置文件结构

> The configuration file consists of directives and their parameters. Simple (single-line) directives each end with a semicolon. Other directives act as “containers” that group together related directives, enclosing them in curly braces ( {} ). Here are some examples of simple directives.

配置文件由指令及其参数组成。简单（单行）指令各自以分号(**`;`**)结尾。其他指令作为“容器”，将相关指令组合在一起，将其包围在花括号（  **`{}`** ）中。以下是简单指令的一些示例。

```
user             nobody;
error_log        logs/error.log notice;
worker_processes 1;
```
# 3. 引用子配置文件
为了使配置更易于维护，可以将一组特定功能的配置文件存储在`/etc/nginx/conf.d`目录中，并使用**`include`**指令将引入`nginx.conf`文件中。
```
include conf.d/http;
include conf.d/stream;
include conf.d/exchange-enhanced;
```

# 4. traffic配置
> A few top-level directives, referred to as contexts, group together the directives that apply to different traffic types:

一些顶级指令，称为上下文，将适用于不同traffic类型的指令组合在一起：
- [events](http://nginx.org/en/docs/ngx_core_module.html?&_ga=2.113298951.1021376016.1495291438-295886557.1494425840#events) – General connection processing
- [http](http://nginx.org/en/docs/http/ngx_http_core_module.html?&_ga=2.113298951.1021376016.1495291438-295886557.1494425840#http) – HTTP traffic
- [mail](http://nginx.org/en/docs/mail/ngx_mail_core_module.html?&_ga=2.113298951.1021376016.1495291438-295886557.1494425840#mail) – Mail traffic
- [stream](http://nginx.org/en/docs/stream/ngx_stream_core_module.html?&_ga=2.51118249.1021376016.1495291438-295886557.1494425840#stream) – TCP traffic

> In each of the traffic-handling contexts, you include one or more server contexts to define virtual servers that control the processing of requests. The directives you can include within a server context vary depending on the traffic type.

在每一个traffic-handling上下文中，包括一个或多个**server**上下文来定义控制和处理虚拟服务的请求。根据不同的traffic在**server**上下文中设置相应指令。

> For HTTP traffic (the http context), each server directive controls the processing of requests for resources at particular domains or IP addresses. One or more location contexts in a server context define how to process specific sets of URIs.

对于HTTP流量（http上下文），每个[server](http://nginx.org/en/docs/http/ngx_http_core_module.html?&_ga=2.84503961.1021376016.1495291438-295886557.1494425840#server)指令控制对特定域名或IP地址上的资源请求的处理。[location](http://nginx.org/en/docs/http/ngx_http_core_module.html?&_ga=2.80406039.1021376016.1495291438-295886557.1494425840#location)上下文中的一个或多个上下文server定义了如何处理特定的URI集合。

> For mail and TCP traffic (the mail and stream contexts) the server directives each control the processing of traffic arriving at a particular TCP port or UNIX socket.

对于邮件和TCP流量（[mail](http://nginx.org/en/docs/mail/ngx_mail_core_module.html?_ga=2.84840217.1286160084.1495291441-295886557.1494425840)和[stream](http://nginx.org/en/docs/stream/ngx_stream_core_module.html?_ga=2.84840217.1286160084.1495291441-295886557.1494425840)上下文），server伪指令各自控制到达特定TCP端口或UNIX套接字的流量处理。

以下配置说明了上下文的使用。
```
user nobody; # a directive in the 'main' context

events {
    # configuration of connection processing
}

http {

    # Configuration specific to HTTP and affecting all virtual servers

    server {
        # configuration of HTTP virtual server 1

        location /one {
            # configuration for processing URIs with '/one'
        }

        location /two {
            # configuration for processing URIs with '/two'
        }
    }

    server {
        # configuration of HTTP virtual server 2
    }
}

stream {
    # Configuration specific to TCP and affecting all virtual servers

    server {
        # configuration of TCP virtual server 1 
    }
}
```

> For most directives, a context that is defined inside another context (a child context) inherits the values of directives included at the parent level. To override a value inherited from the parent, include the directive in the child context. For more information on context inheritence, see the documentation for the [proxy_set_header](http://nginx.org/en/docs/http/ngx_http_proxy_module.html?&_ga=2.9808053.1286160084.1495291441-295886557.1494425840#proxy_set_header) directive.

对于大多数指令，在另一个上下文（子上下文）中定义的上下文将继承父级中包含的伪指令的值。要覆盖从父进程继承的值，请在子上下文中包含该指令。有关上下文遗留的更多信息，请参阅指令的[proxy_set_header](http://nginx.org/en/docs/http/ngx_http_proxy_module.html?&_ga=2.9808053.1286160084.1495291441-295886557.1494425840#proxy_set_header)文档。

# 5. 生效修改的配置
要使更改修改过后的配置文件生效，Nginx必须重新加载文件。方式有两种：
- reload方式（推荐使用）

    使用reload signal来生效修改后的配置，而不会中断当前请求的处理。
    ```
    # nginx -s reload
    ```
- 重启Nginx

    步骤：

    - 关闭Nginx
    ```
    # nginx -s stop
    ```
    - 启动Nginx
    ```
    # nginx -c /etc/nginx/nginx.conf        
    ```

# 6. 参考文献
[CREATING NGINX PLUS CONFIGURATION FILES](https://www.nginx.com/resources/admin-guide/configuration-files/)