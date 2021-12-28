---
title: 在树莓派4b上安装debian
categories:
  - 硬件
tags:
  - raspberry
  - debian
author: geepair
date: 2021-12-27 22:25:00
---

在树莓派4b安装高性能的Debian做服务器，地址：[Debian-Pi-Aarch64](https://gitee.com/openfans-community/Debian-Pi-Aarch64)


<!-- more -->

> 本次系统完全不同于之前我们所发布的所有系统(同样也包括之前的64位的Debian )，此次我们全部从头全新构建。在我们的实验室新构建了全新的自动编译和打包、测试系统，同样我们也对系统重新定义了打包流程和调整了所有的相关配置，对内核进行了大量的修改、调整优化和BUG修复，加入了很多新的功能和特性，特别是加入了KVM虚拟化的支持以及重点加强了对Docker的各项特性支持和优化。

兼容诸多特性：
  - 支持64位aarch64架构，官网默认只支持32位armhf架构
  - 原生的有线、无线网卡、蓝牙、3D加速均可正常使用，所有系统软件包数量几乎可以媲美X86的版本，系统基于原生 Debian 64位 从头构建（非任何移植版和官改版本），保证原滋原味
  - ...