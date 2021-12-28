---
title: idea创建新项目时默认设置会自动恢复
categories:
  - 后端
tags:
  - IntellJ IDEA
author: geepair
date: 2020-03-29 18:44:00
---

在使用idea新建一个项目的时候，例如一些设置会自动恢复到默认的状态，比如modules的jdk等级，maven的默认设置文件和默认仓库等。这些都可以通过设置改变。遇到此问题，记录一下。

<!-- more -->

- 找到idea的标签第一个file找到Other Settings->Settings for new Projects.就可以改变项目的默认设置，每次新建一个新项目就会使用这些设置了。

![pasted-0](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106144419.png)

![pasted-1](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106144505.png)

- 设置Structure可以修改默认的modules的jdk语言等级

![pasted-2](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106144539.png)

![pasted-3](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106144557.png)

- 最后保存就可以啦。