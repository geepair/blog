---
title: 使用hexo创建自己的个人博客
categories:
  - 后端
tags:
	- Hexo
author: geepair
date: 2020-03-14 19:19:00
---

在Linux上使用 hexo 来搭建自己的博客
<!--more-->

## hexo 官方文档

```shell
$ npm install hexo-cli -g
$ hexo init blog
$ cd blog
$ npm install
$ hexo server
```

#### 安装步骤

- 安装 nodejs ( Centos 使用 yum 命令)
hexo 必须配合 node 和 git 才能运行，不低于 Node.js 8.10，建议 10.0 或以上，如果运  行hexo < command > 出错，可能是因为 node 的版本过低导致的。

    ```shell
    sudo yum install nodejs
    sudo yum install npm
    sudo yum install git-core
    ```

- 安装 hexo (-g全局安装)

    ```shell
    npm install -g hexo-cli
    ```

- 初始化博客的位置(自己新建一个文件夹XXX)，名字自己取，使用 `hexo init` 命令初始化，进入到自己创建的目录中。

    ```shell
    mkdir hexoblog
    hexo init hexoblog
    cd hexoblog
    ```

- 生成静态文件并运行服务。

    `source`-->`_post` 文件夹存储的是自己提交的文件
    
    ```shell
    hexo g
    hexo server
    ```

- 可以通过域名和端口号(默认4000)访问了，服务器记得安全组开启端口

    ```http
    http://localhost:4000
    ```