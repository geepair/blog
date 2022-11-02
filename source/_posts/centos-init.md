---
title: Centos 初始化配置
categories:
  - 后端
  - 服务器
tags:
  - CentOS
  - Linux
  - Mirror
author: geepair
date: 2022-06-08 23:11:00
---

CentOS服务器初始化配置

<!-- more -->

#### 更新centos依赖，地址：[清华大学开源镜像站](https://mirrors.tuna.tsinghua.edu.cn/) | [CentOS](https://mirrors.tuna.tsinghua.edu.cn/help/centos/)


该文件夹只提供 CentOS 7 与 8，架构仅为 x86_64 ，如果需要较早版本的 CentOS，请参考 centos-vault 的帮助，若需要其他架构，请参考 centos-altarch 的帮助。

建议先备份 /etc/yum.repos.d/ 内的文件。

然后编辑 /etc/yum.repos.d/ 中的相应文件，在 mirrorlist= 开头行前面加 # 注释掉；并将 baseurl= 开头行取消注释（如果被注释的话）。 对于 CentOS 7 ，请把该行内的域名（例如`mirror.centos.org`）替换为 `mirrors.tuna.tsinghua.edu.cn`。 对于 CentOS 8 ，请把 `mirror.centos.org/$contentdir` 替换为 `mirrors.tuna.tsinghua.edu.cn/centos`。

- 以上步骤可以被下方的命令一步完成

  ```shell
  # 对于 CentOS 7
  sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
          -e 's|^#baseurl=http://mirror.centos.org|baseurl=https://mirrors.tuna.tsinghua.edu.cn|g' \
          -i.bak \
          /etc/yum.repos.d/CentOS-*.repo

  # 对于 CentOS 8
  sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
          -e 's|^#baseurl=http://mirror.centos.org/$contentdir|baseurl=https://mirrors.tuna.tsinghua.edu.cn/centos|g' \
          -i.bak \
          /etc/yum.repos.d/CentOS-*.repo
  ```

- 最后，更新软件包缓存

  ```shell
  sudo yum makecache
  ```

#### 安装nginx服务，地址：[Tengine](https://tengine.taobao.org/) [nginx安装和性能优化](./nginx-optimization.md)

#### CentOS开启BBR，地址：[开启BBR](https://www.jianshu.com/p/09069a354f02)

#### Sysctl.conf配置备份

```shell
fs.file-max = 999999
net.ipv4.ip_forward = 0
net.ipv4.conf.default.rp_filter = 1
net.ipv4.conf.default.accept_source_route = 0
kernel.sysrq = 0
kernel.core_uses_pid = 1
kernel.msgmnb = 65536
kernel.msgmax = 65536
kernel.shmmax = 68719476736
kernel.shmall = 4294967296
net.ipv4.tcp_max_tw_buckets = 6000
net.ipv4.tcp_sack = 1
net.ipv4.tcp_window_scaling = 1
net.ipv4.ip_local_port_range=1024 65000
net.ipv4.tcp_synack_retries = 1
net.ipv4.tcp_syn_retries = 1
net.ipv4.tcp_tw_recycle = 1
net.ipv4.tcp_tw_reuse = 1
net.ipv4.tcp_keepalive_time = 30
net.ipv4.tcp_syncookies = 1
net.core.somaxconn = 40960
net.ipv4.tcp_max_orphans = 3276800
net.ipv4.tcp_timestamps = 0
net.core.netdev_max_backlog = 262144
net.ipv4.tcp_max_syn_backlog = 262144
net.ipv4.tcp_rmem = 10240 87380 12582912
net.ipv4.tcp_wmem = 10240 87380 12582912
net.core.rmem_default = 8388608
net.core.wmem_default = 8388608
net.core.rmem_max = 16777216
net.core.wmem_max = 16777216
net.ipv4.tcp_mem = 94500000 915000000 927000000
net.ipv4.tcp_fin_timeout = 1
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```

#### 修改打开文件数量
`ulimit -n`