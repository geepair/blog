---
title: Nginx配置优化
categories:
  - 后端
tags:
  - Nginx
author: geepair
date: 2022-01-03 15:43:00
---

nginx配置文件优化详解

<!-- more -->

# Nginx需要做一些优化配置

## nginx.conf文件

```bash
#user  nobody;
worker_processes  auto; # cpu核心数
worker_cpu_affinity auto; # 0001 0010 0100 1000 散列
worker_rlimit_nofile 1024; # 文件打开数量

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;
#error_log  "pipe:rollback logs/error_log interval=1d baknum=7 maxsize=2G";

#pid        logs/nginx.pid;

events {
    use epoll; # epoll事件处理
    worker_connections 1024; # 可处理的连接数
    multi_accept on; # 多个进程同步启动
}

http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;
    #access_log  "pipe:rollback logs/access_log interval=1d baknum=7 maxsize=1G"  main;

    sendfile        on; # 
    tcp_nopush     on; # 多个tcp合并

    keepalive_timeout  60;
    tcp_nodelay on;
    client_header_buffer_size 4k; # 头部大小
    open_file_cache max=102400 inactive=20s;
    open_file_cache_valid 30s;
    open_file_cache_min_uses 1;
    client_header_timeout 15;
    client_body_timeout 15;
    reset_timedout_connection on; # 超时重连
    send_timeout 15;
    server_tokens off; # 隐藏版本号
    client_max_body_size 10m; # 上传文件大小限制

    gzip  on; # 打开压缩，消耗cpu
    gzip_min_length 1k;
    gzip_buffers 4 32k;
    #gzip_http_version 1.1;
    gzip_comp_level 6; # 压缩等级 1-9
    gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
    gzip_vary on;
    gzip_proxied any;
    
    # ssl
    ssl_certificate cert.crt;
    ssl_certificate_key cert.key;
    ssl_session_cache shared:SSL:10m;
    ssl_session_timeout 1d;
    ssl_session_tickets off;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
    ssl_prefer_server_ciphers off;
    #ssl_stapling on;
    #ssl_stapling_verify on;

    # 上游服务，可做负载均衡
    upstream pi {
        server 127.0.0.1:9090;
    }
    
    # 重定向80->443
    server {
        listen 80;
        server_name ijava.me www.ijava.me pi.ijava.me;
        resolver 114.114.114.114;
        return 301 https://$host$request_uri;
    }

    # HTTPS server
    server {
        listen       443 ssl http2;
        server_name  ijava.me www.ijava.me;
        ssl_certificate      ijava_me.crt;
        ssl_certificate_key  ijava_me.key;
        ssl_session_cache    shared:SSL:10m;
        ssl_session_timeout  1d;
        ssl_session_tickets off;
        ssl_protocols TLSv1.1 TLSv1.2;
        ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
        ssl_prefer_server_ciphers  off;
        #ssl_stapling on;
        #ssl_stapling_verify on;

        resolver 114.114.114.114;
        # 静态文件访问
        location / {
            root   /home/pi/html;
            index  index.html index.htm;
            charset utf-8;
        }
        # 连接数状态统计module
        location /status {
            stub_status on;
        }
    }
    
    server {
        listen 443 ssl http2;
        server_name pi.ijava.me;
        resolver 114.114.114.114;
        # 反向代理
        location / {
            proxy_pass http://pi;
            proxy_set_header X-Forwared-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_redirect off;
        }
        # 代理开启wss支持
        location /cockpit/socket {
            proxy_pass https://pi;
            proxy_redirect off;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
            proxy_set_header Host $host;
            proxy_set_header X-Real-IP $remote_addr;
            proxy_http_version 1.1;
            proxy_set_header Upgrade $http_upgrade;
            proxy_set_header Connection "upgrade";
        }

        # 图片类型文件缓存
        location ~* \.(ico|jpe?g|gif|png|bmp|swf|flv)$ {
            proxy_pass http://pi;
            expires 30d;
            #log_not_found access_log off;
            access_log off;
        }
        # js/css文件缓存
        location ~* \.(js|css)$ {
            proxy_pass http://pi;
            expires 7d;
            log_not_found off;
            access_log off;
        }
    }
}
```

## sysctl.conf文件

```bash
net.core.default_qdisc=fq
net.ipv4.tcp_congestion_control=bbr

# shutdown ipv6
net.ipv6.conf.all.disable_ipv6=1

# optimiaze
fs.file-max = 999999
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
kernel.sysrq = 0
kernel.core_uses_pid = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.shmmax = 68719476736
kernel.shmall = 4294967296
net.ipv4.tcp_max_tw_buckets = 6000
net.ipv4.tcp_sack = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.ip_local_port_range=1024 65000
net.ipv4.tcp_synack_retries = 1
net.ipv4.tcp_syn_retries = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_keepalive_time = 30
net.ipv4.tcp_syncookies = 1
net.core.somaxconn = 40960
net.ipv4.tcp_max_orphans = 3276800
net.ipv4.tcp_timestamps = 0
net.core.netdev_max_backlog = 262144
net.ipv4.tcp_max_syn_backlog = 262144
net.ipv4.tcp_rmem = 10240 87380 12582912
net.ipv4.tcp_wmem = 10240 87380 12582912
net.core.rmem_default = 8388608
net.core.wmem_default = 8388608
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_mem = 94500000 915000000 927000000
net.ipv4.tcp_fin_timeout = 1
```