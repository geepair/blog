---
title: Linux替换默认的OpenJDK为OracleJDK
categories:
  - 后端
tags:
  - Linux
  - Java
  - Tool
author: geepair
date: 2021-06-22 22:55:00
---

 Linux刚安装时一般不自带JDK或者是自带了开源的OpenJDK，自己使用的一般替换为OracleJDK，需要把自带的`OpenJDK`卸载，再安装上`OracleJDK`

- 以CentOS为例，首先我们需要把自带的JDK删除

  1.查看已经安装的OpenJDK版本

  ```bash
  java -version # dispaly version
  ```

  ![image-20210622221048765](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/22/20210622221048.png)

  2.查看本机已经安装的有关java的包，可以选择卸载

  ```bash
  rpm -qa | grep java # query java package
  ```

  ![image-20210622221725990](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/22/20210622221726.png)

  使用`rpm -e `卸载包，以noarch结尾的可以不删除，再次使用`java -version`此时就失效了

  ```bash
  rpm -e --nodeps java-1.8.0-openjdk-headless-1.8.0.292.b10-1.el8_4.x86_64 # rpm
  yum -y remove *openjdk* # yum
  ```

- 安装OracleJDK，从Oracle官网下载好包，解压安装

  1.下载OracleJDK包，Linux 压缩包x64 [OralceJDK 8](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)

  ![image-20210622223017297](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/22/20210622223017.png)

  2.解压下载好的压缩包，创建Java文件夹复制过去，配置好环境变量

  ```bash
  tar -zxvf jdk-8u291-linux-x64.tar.gz # tar
  mkdir -p /usr/local/java
  cp -r jdk1.8.0_291/* /usr/local/java
  
  vim /etc/profile
  export JAVA_HOME="/usr/local/java" # java root project
  export JRE_HOME="${JAVA_HOME}/jre"
  export CLASSPATH=".:${JAVA_HOME}/lib:${JRE_HOME}/lib"
  export PATH="${JAVA_HOME}/bin:${PATH}"
  
  source /etc/profile # refresh profile
  java -version # display version
  ```

  ![image-20210622225043692](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/22/20210622225043.png)