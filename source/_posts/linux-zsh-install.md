---
title: Linux上ZSH安装并配置on-my-zsh插件
categories:
  - 后端
tags:
  - Shell
  - Linux
author: geepair
date: 2021-06-22 21:47:00
---

 记录Linux上`ZSH`相关的配置，从仓库安装配置并配置相关的插件，美化终端使之更容易操作。使用二次开发的[on-my-zsh](https://ohmyz.sh/)

- 安装`zsh`，使用自带的包管理工具

  ```bash
  apt install zsh # for Ubuntu
  yum install zsh  # for CentOS，dnf replace yum also
  brew install zsh # MacOS
  ```

- 安装`on-my-zsh`，克隆oh-my-zsh仓库或者使用安装脚本

  ```bash
  wget https://gitee.com/mirrors/oh-my-zsh/raw/master/tools/install.sh
  ```

- 配置`zsh`，修改主题以及添加插件`zsh-syntax-highlighting`，`zsh-autosuggestions`

  ```bash
  vim ~/.zshrc # root 
  ZSH_THEME="robbyrussell" # change, like "ys"
  plugins=(git zsh-syntax-highlighting zsh-autosuggestions) # search plugins and add
  source ~/.bash_profile
  ```

  1. 安装插件[zsh-syntax-highlighting](https://github.com/zsh-users/zsh-syntax-highlighting)

     ```bash
     git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
     ```

  2. 安装插件[zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions)

     ```bash
     git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
     ```

- 刷新zsh配置，配置完成

  ```bash
  source ~/.zshrc
  ```
  

![](https://cdn.jsdelivr.net/gh/geepair/picgo@master/img/2021/06/22/20210622215602.png)
