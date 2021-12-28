---
title: 使用github的github pages搭建静态的网站
categories:
  - 前端
tags:
  - GithubPages
author: geepair
date: 2020-05-12 22:19:00
---

使用 github 的 `github pages` 搭建静态的网站，使用自己的域名并使用cloudflare开启CDN加速。

<!-- more -->

#### 新建GitHub仓库开启GitHub Pages
- 先新建一个GitHub仓库，用来储存网站的页面文件`必须是公有的仓库!`
  建立好了之后可以克隆到本地，方便进行页面修改。

- 开启GitHub Pages
  进入仓库的设置页面`Settings`->`OPtions`->`GitHub Pages`选中一个分支，仓库不能为空，选中作为GitHub Pages必须是主分支或者主分支下的/docs文件夹。之后就可以通过 `https://用户名.github.io/项目名`来访问了。
  
  ![pasted-18](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106154436.png)

#### 绑定自己的域名

- 在 `GitHub Pages`选项中填入自己的自定义域名，在这之前必须开启DNS解析，在自己的域名服务商，添加一个 CNAME解析记录 到 `用户名.github.io` 或者直接添加一个A记录解析到 IP 地址（自行百度）

- 可以选择开启HTTPS，一开始可能不能启用，过段时间会给这个域名自动签发证书。

  ![pasted-19](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106155321.png)

#### 使用 Cloudflare CDN 加速

- 添加自己的域名，需要修改原有的 Name Server，等待生效，也可以直接在DNS修改CNAME记录

- 开启 SSL/TLS。