---
title: Centos 安装 Redis 服务
categories:
  - 后端
tags:
  - Redis
  - CentOS
author: geepair
date: 2019-03-14 19:05:00
---

在Centos上安装Redis服务端，并使用客户端连接

<!--more-->

#### 使用 yum 命令安装一键安装 Redis

```shell
yum install redis
```

#### 打开 redis 的服务

```shell
systemctl start redis.service
```

#### 开启远程访问权限，要修改redis的配置文件，配置文件的默认的路径是 `/etc/redis.conf`,

```shell
# 默认只允许本机客户端访问
bind 127.0.0.1 # 找到这一行 
# 把这一行注释掉
```
![20200311001](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151500.png)

```shell
# 关闭保护模式，保护模式会防止远程访问
protected-mode yes # 找到
# 把 yes 更改为 no
```
![20200311002](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151705.png)

```bash
# 修改redis的密码，redis 默认是没有设置密码的
requirepass foobard # 找到
# 打开他的注释，删掉 #，foobard 改成自己设置的密码
```
![20200311003](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151716.png)

```bash
# 最后记得重启服务
systemctl restart redis.service
```

#### 设置了防火墙的话记得放行 `6379` 端口（redis的端口），如果用的是云服务器，在安全组中添加 `6379` 端口号的入口