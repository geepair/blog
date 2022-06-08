---
title: Tengine编译安装
categories:
  - 后端
tags:
  - Nginx
author: geepair
date: 2021-06-30 17:05:00
---

> [Tengine](https://tengine.taobao.org/)是由淘宝网发起的Web服务器项目。它在[Nginx](http://nginx.org/)的基础上，针对大访问量网站的需求，添加了很多高级功能和特性。[Tengine](https://tengine.taobao.org/)的性能和稳定性已经在大型的网站如[淘宝网](http://www.taobao.com/)，[天猫商城](http://www.tmall.com/)等得到了很好的检验。它的最终目标是打造一个高效、稳定、安全、易用的Web平台。

把Tengine编译安装在CentOS服务器上

#### 首先我们从[下载地址](https://tengine.taobao.org/download/tengine-2.3.3.tar.gz)获取到tengine的安装包

    ```bash
    wget https://tengine.taobao.org/download/tengine-2.3.3.tar.gz # change version
    ```

#### 开始安装Tengine，之前需要安装必要的软件包`gcc`, `gcc-c++`, `pcre-devel`, `openssl-devel`等

  ```shell
  mkdir -p /usr/local/nginx # custom root library
  tar -zxvf tengine-2.3.3.tar.gz && cd tengine-2.3.3 # tar and goto
  ```

  ```shell
  ./configure \
  > --prefix=/usr/local/nginx/
  > --conf-path=/etc/nginx/nginx.conf \
  > --with-http_v2_module
  ... # edit oll property and module
  ```

  ```shell
  make && make install
  ```

#### 进入到Tengine的根目录运行，显示少了文件目录则重新创建

  ```shell
  cd /usr/local/nginx
  ./sbin/nginx
  ```

#### 安装完成，可以通过网页80端口进行访问，也可修改`conf/nginx.conf`文件修改Tengine的配置

  `curl -i localhost`

  ![image-20210630175145955](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/30/20210630175152.png)

#### 做个链接

  ```shell
  ln -s /usr/local/nginx/sbin/nginx /bin/nginx
  ```

#### 添加服务

  ```c
  // nginx.service
  [Unit]
  Description=Nginx - high performance web server
  After=network.target remote-fs.target nss-lookup.target

  [Service]
  Type=forking
  #User=nobody
  ExecStart=/usr/bin/nginx -c /etc/nginx/nginx.conf
  ExecReload=/usr/bin/nginx -s reload
  ExecStop=/usr/bin/nginx -s stop

  [Install]
  WantedBy=multi-user.target
  ```

  #### 贴一下配置文件

  ```c
  user  root;
  worker_processes  auto;
  worker_cpu_affinity auto;
  worker_rlimit_nofile 65535;

  #error_log  logs/error.log;
  #error_log  logs/error.log  notice;
  #error_log  logs/error.log  info;
  #error_log  "pipe:rollback logs/error_log interval=1d baknum=7 maxsize=2G";

  #pid        logs/nginx.pid;


  events {
      use epoll;
      worker_connections 65535;
      #multi_accept on;
  }


  http {
      include       mime.types;
      default_type  application/octet-stream;

      #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
      #                  '$status $body_bytes_sent "$http_referer" '
      #                  '"$http_user_agent" "$http_x_forwarded_for"';

      #access_log  logs/access.log  main;
      #access_log  "pipe:rollback logs/access_log interval=1d baknum=7 maxsize=1G"  main;

      sendfile        on;
      tcp_nopush     on;

      keepalive_timeout  60;
      tcp_nodelay on;
      client_header_buffer_size 4k;
      open_file_cache max=102400 inactive=20s;
      open_file_cache_valid 30s;
      open_file_cache_min_uses 1;
      client_header_timeout 15;
      client_body_timeout 15;
      reset_timedout_connection on;
      send_timeout 15;
      server_tokens off;
      client_max_body_size 1g;

      gzip  on;
      gzip_min_length 1k;
      gzip_buffers 4 32k;
      #gzip_http_version 1.1;
      gzip_comp_level 6;
      gzip_types text/plain text/css text/javascript application/json application/javascript application/x-javascript application/xml;
      gzip_vary on;
      gzip_proxied any;
      
      # ssl
      ssl_certificate cert.crt;
      ssl_certificate_key cert.key;
      ssl_session_cache shared:SSL:10m;
      ssl_session_timeout 5m;
      ssl_session_tickets on;
      ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
      ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
      ssl_prefer_server_ciphers on;
      ssl_stapling on;
      ssl_stapling_verify on;

      resolver 114.114.114.114;

      
      server {
          listen 80;
          server_name www.ijava.me s.ijava.me;
          return 301 https://$host$request_uri;
      }

      server {
          listen 80 default;
          listen 443 ssl http2 default;
          rewrite ^(.*) https://www.ijava.me;
      }

      server {
          listen 443 ssl http2;
          server_name s.ijava.me;
          charset utf-8;
          location / {
              root /root/data/static;
              autoindex on;
              autoindex_exact_size on;
              autoindex_format html;
              autoindex_localtime on;
              set $limit_rate 1M;
          }
      }

      server {
          listen 443 ssl http2;
          server_name www.ijava.me;

          ssl_certificate      ijava_me.crt;
          ssl_certificate_key  ijava_me.key;
          ssl_session_cache    shared:SSL:10m;
          ssl_session_timeout  5m;
          ssl_session_tickets on;
          ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
          ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:DHE-RSA-AES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384;
          ssl_prefer_server_ciphers  on;
          #ssl_stapling on;
          #ssl_stapling_verify on;

          location / {
              root   html;
              index  index.html index.htm;
              charset utf-8;
          }

          location /status {
              stub_status on;
          }
      }

  }
  ```

 