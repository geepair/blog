---
title: 开启Mysql远程访问
catagories: 
  - 后端
tags: 
  - Linux
  - MySQL
author: geepair
date: 2019-03-14 15:56:00
---

linux上开启一个新的临时账户用来远程连接Mysql，使用3306端口

<!-- more -->

### 开启Mysql远程访问

- 在服务器上自带的客户端连接到mysql的命令行，输入`root`用户的密码

```shell
mysql> mysql -u root -p
mysql> select user,host from mysql.user;
```

- 直接`root`远程访问连接

```shell
# 方法一
mysql> update mysql.user set host='%' where user='root';
# 方法二（推荐使用）
mysql> GRANT ALL PRIVILEGES ON *.* TO '账号' @ '%' IDENTIFIED BY '密码' WITH GRANT OPTION;
```

- 退出客户端

```shell
mysql> exit
```

- 重启mysql服务

```shell
systemctl restart mysql.service
# 或者
service mysql restart
```

- 开启端口3306的访问权限