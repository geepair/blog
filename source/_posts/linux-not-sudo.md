---
title: Linux创建的用户不能使用 sudo 命令
catagories: 
  - 后端
tags: 
  - Linux
author: geepair
date: 2020-04-07 18:46:00
---

使用 useradd 新建的用户无法使用root命令,开启控制功能

<!-- more -->

- 先切换到`root`用户

```shell
su root
```

- 添加`sudoers`文件的写w的权限

```shell
chmod u+w /etc/sudoers
```

- 编辑`sudoers`文件

```shell
vi /etc/sudoers
```

- 找到一行 `root ALL=(ALL) ALL`，在下面添加用户 user (ps：你自己的用户名)

```shell
user ALL=(ALL) ALL # user用户可以使用sudo命令的权限
%user ALL=(ALL) ALL # user用户组的所有用户可以使用sudo命令的权限
user ALL=(ALL) NOPASSWD:ALL # user用户可以使用sudo命令的权限，并且使用时不需要密码
%user ALL=(ALL) NOPASSWD:ALL # user用户组的所有用户可以使用sudo命令的权限，并且使用时不需要密码
```

- 最后，撤销sudoers文件的写权限

```shell
chmod u-w /etc/sudoers
```