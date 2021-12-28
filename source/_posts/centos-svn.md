---
title: CentOS 上配置 SVN 版本管理工具
categories:
  - 后端
tags:
  - SVN
  - CentOS
author: geepair
date: 2019-03-14 19:08:00
---

在Liunx上搭建SVN服务端来进行客户端的连接，进行多人协作，版本控制的功能。
<!--more-->

#### 使用 `yum` 命令直接安装 `SVN`



```shell
yum install -y subversion
```
![20200311004](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106150740.png)

检查时候安装成功，安装后默认的路径是 `/usr/bin/svnserve`，使用命令 `svnserve --version` 查看安装是否成功和安装版本

#### 创建SVN版本库

```shell
mkdir -p /data/svn/test (自定义目录，递归创建)
svnadmin create /data/svn/test
```

ps: 可以通过修改 `/etc/sysconfig/svnserve` 文件来改变默认svn仓库的位置，启动时就不需要把仓库路径写出来了

```bash
OPTIONS="-r /data/svn"    # 默认是/var/svn
```
![20200311005](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106150841.png)

`conf` 文件夹内有此版本库的配置文件

#### 配置SVN信息

```shell
cd /data/svn/test/conf
```

在 conf 文件夹内，有3个文件，分别是
* `authz` 管理权限
* `passwd` 管理用户账号密码
* `svnserve.conf` 版本库配置文件

```bash
# authz文件
[groups]
...
admin = admin,admin2    #管理员组
guest = guest1,guest2    #用户组
...
[/]
...
@admin = rw    #管理员组拥有读写权限
@guest = r    #用户组读权限
* =     #其他
...
```

```bash
# passwd文件
[users]     #用户账号和密码
admin = 123456
guest1 = 123
```

```bash
# svnserve.conf文件,把其中的一些注释打开
[general]   #权限有 read, write, none
anon-access = none    #匿名用户没有任何权限
...
auth-access = write    #使授权用户有写的权限
...
password-db = passwd    #密码文件的位置
...
authz-db = authz    #鉴权文件的位置
```

#### 启动 SVN 服务

- 服务端启动 `SVN` 服务，默认是 `3690` 端口，防火墙记得打开，或者在后面加上 `--listen-port` 使用特定端口

```shell
svnserve -d -r /data/svn  --listen-port 3690    #版本库位置是/data/svn 可以改变访问监听端口
# 或者
systemctl start svnserve.service    #这是修改了默认svn存储路径是可以这样做，否则会报错，默认仓库不存在
```

- 使用客户端连接，使用 TortioseSVN GUI 图形化界面连接

1. 新建一个文件夹用来存储版本库的位置，右键 `Check out` 检出URL中包含 svn://(协议) 127.0.0.1(主机地址) /test(版本库名，这里就不用加/data/svn了)

![20200311006](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151003.png)

2. 填写用户名密码就能使用其他操作了。

![20200311007](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151016.png)

![20200311008](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106151036.png)