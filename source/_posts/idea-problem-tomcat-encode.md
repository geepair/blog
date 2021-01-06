---
title: idea中web项目使用tomcat乱码的问题
categories:
  - 工具
tags:
  - IntellJ IDEA
author: geepair
date: 2020-04-07 14:01:00
---

使用idea嵌入外部的tomcat，在服务运行的时候控制台运行情况会产生乱码

<!-- more -->

# idea使用Tomcat控制台乱码

#### 2019版之前的版本

- 方法一
打开这两个文件并再文件的后面加上 `-Dfile.encoding=UTF-8`

![pasted-4](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153117.png)

![pasted-5](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153128.png)

- 方法二
找到 `tomcat` 的安装目录，修改日志文件的配置，把所有的编码格式改为GBK

![pasted-6](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153145.png)

![pasted-7](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153153.png)

#### 2019版之后的版本

- 方法

把全局设置编码UTF-8,并修改 `VMOption`

![pasted-8](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153216.png)

在上边help选项卡，找到 `setting VMOption`,同样加上 `-Dfile.encoding=UTF-8`,重新启动idea就可以了

![pasted-9](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153230.png)

![pasted-10](https://cdn.jsdelivr.net/gh/geepair/PicGo/img/2021/01/06/20210106153241.png)


