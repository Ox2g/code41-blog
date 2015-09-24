title: Node普通应用的基本Nginx配置
date: 2015-09-24 22:41:11
tags:
- nginx
- 基础
- 反向代理
categories:
- 基础
- nginx
---

## Node普通应用的基本Nginx配置

> 前几日由于网慢，自己购买了一个小的docker容器，这两天就打算鼓捣个小的应用放到上面，同时配置上自己之前的域名，但是在端口的出现毕竟不好看，之前一直知道Nginx的用途，故配置了一个小的Nginx的反向代理来实践一下。

#### Centos 安装Nginx

```
# 安装nginx需要的一些第三方包，来支持相关模块的使用
yum -y install gcc gcc-c++ autoconf automake make
yum -y install zlib zlib-devel openssl openssl--devel pcre pcre-devel 

# 安装nginx
yum install nginx
```

<!-- more -->

#### nginx的基本操作

```
# 启动 前面是命令，后面参数为配置文件
/usr/sbin/nginx -c /etc/nginx/nginx.conf
# 停止和重启 TODO:待补充实践，目前测试不成功

# 强制删除全部nginx相关进程
kill -9 PIDS
```

#### 配置文件描述基本配置

- 主配置文件部分

```
#nginx 运行的用户和所在组
ser noder web;
#辅助的worker进程数，一般为总核数 2核4处理器 => 8
worker_processes auto;
#日志打印位置 可以追加日志级别
error_log /var/log/nginx/error.log;
#nginx运行的进程ID
pid /run/nginx.pid;
#当一个nginx 进程打开的最多文件描述符数目
worker_rlimit_nofile                    65535;
#nginx的核心模块之一，后续进阶文章讲解
events
{
                                        use epoll;
                                        worker_connections 65535;
}
#nginx核心模块之一，主要处理网络相关
http
{
        include                         mime.types;
        default_type                    application/octet-stream;
        server_tokens                   on;
        log_format main                 '$remote_addr - $remote_user [$time_local] '
                                                        '"$request" $status $bytes_sent '
                                                        '"$http_referer" "$http_user_agent" '
                                                        '"$gzip_ratio"';
        #charset                        utf-8;
        server_names_hash_bucket_size   128;
        client_header_buffer_size       32k;
        large_client_header_buffers     4 32k;
        client_max_body_size            300m;
        sendfile                        on;
        tcp_nopush                      on;
        keepalive_timeout               0;
        tcp_nodelay                     on;
        client_body_buffer_size         512k;
        fastcgi_intercept_errors        on;
        proxy_connect_timeout           90;
        proxy_read_timeout              180;
        proxy_send_timeout              180;
        proxy_buffer_size               256k;
        proxy_buffers                   4 256k;
        proxy_busy_buffers_size         256k;
        proxy_temp_file_write_size      256k;
        proxy_intercept_errors          on;
        server_name_in_redirect         off;
        proxy_hide_header       X-Powered-By;

        #gzip压缩相关
        gzip                            on;
        gzip_min_length                 100;
        gzip_buffers                    4 16k;
        gzip_http_version               1.0;
        gzip_comp_level                 9;
        gzip_types                      text/plain application/x-javascript text/css application/xml;
        gzip_vary                       on;
        error_page 400 401 402 403 404 405 408 410 412 413 414 415 500 501 502 503 506 = http://code41.me/;

#模块化处理反向代理文件，拆分不同域名独立文件
include domains/*;
# 模块话内容的主要组成
###########status#########
#        server
#                {
#                 listen                 80;
#                 server_name            status.code41.me;
#        location / {
#                 stub_status            on;
#                 access_log             off;
#                 }
#        }
}
```

- 域名反向代理配置

```
#代理最终请求的地址，nginx模块upstream的基本用法
upstream noder_web {
                server 127.0.0.1:3000  weight=10 max_fails=2 fail_timeout=30s;
                }
#核心服务配置部分
server
{
    #监听端口
    listen                   80;
    #域名的虚拟主机名称
    server_name              www.code41.me code41.me cms.code41.me;
    #请求来源日志
    access_log               /var/log/nginx/domains/noder_web_access.log main;
    #请求处理日志
    error_log                /var/log/nginx/domains/noder_web_error.log error;
    location / {
        proxy_next_upstream     http_500 http_502 http_503 http_504 error timeout invalid_header;
        proxy_set_header        Host  $host;
        proxy_set_header        X-Forwarded-For $proxy_add_x_forwarded_for;
        #代理请求
        proxy_pass              http://noder_web;
        expires                 1d;
        }
    location /logs/ {
                autoindex       off;
                deny all;
        }
}
```
