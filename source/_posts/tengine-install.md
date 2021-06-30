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

- 首先我们从[下载地址](https://tengine.taobao.org/download/tengine-2.3.3.tar.gz)获取到tengine的安装包

    ```bash
    wget https://tengine.taobao.org/download/tengine-2.3.3.tar.gz # change version
    ```

- 开始安装Tengine，之前需要安装必要的软件包`gcc`, `gcc-c++`, `pcre-devel`, `openssl-devel`等

  ```bash
  mkdir -p /usr/local/nginx # custom root library
  tar -zxvf tengine-2.3.3.tar.gz && cd tengine-2.3.3 # tar and goto

  ./configure \
  > --prefix=/usr/local/tengine/ \
  > --error-log-path=/var/log/tengine/error.log \
  > --http-log-path=/var/log/tengine/access.log \
  > --pid-path=/var/run/tengine/tengine.pid \
  > --lock-path=/var/lock/tengine.lock \
  > --with-http_ssl_module \
  > --with-http_flv_module \
  > --with-http_stub_status_module \
  > --with-http_gzip_static_module \
  > --http-client-body-temp-path=/var/tmp/tengine/client/ \
  > --http-proxy-temp-path=/var/tmp/tengine/proxy/ \
  > --http-fastcgi-temp-path=/var/tmp/tengine/fcgi/ \
  > --http-uwsgi-temp-path=/var/tmp/tengine/uwsgi \
  > --http-scgi-temp-path=/var/tmp/tengine/scgi \
  > --with-pcre
  ... # edit oll property and module
  
  make && make install
  ```
  

- 进入到Tengine的根目录运行，显示少了文件目录则重新创建

  ```bash
  cd /usr/local/nginx
  ./sbin/nginx
  curl -i localhost
  ```

  ![image-20210630175145955](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/30/20210630175152.png)

- 安装完成，可以通过网页80端口进行访问，也可修改`conf/nginx.conf`文件修改Tengine的配置