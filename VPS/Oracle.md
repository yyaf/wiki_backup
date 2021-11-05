---
title: Oracle
description: 
published: true
date: 2021-11-05T07:40:10.169Z
tags: vps
editor: markdown
dateCreated: 2021-11-05T06:50:39.464Z
---

# Oracle Cloud

甲骨文家的免费实例。

## 一、Ubuntu 20 dd Debian

```bash
sudo -i

wget -N --no-check-certificate https://raw.githubusercontent.com/veip007/dd/master/InstallNET.sh && chmod +x InstallNET.sh && ./InstallNET.sh -d 10 -v 64 -p "root密码" -port "端口"
```
> 引用萌咖大佬的一键DD脚本：https://github.com/veip007/dd

## 二、更新到5.10内核

1. 下载内核  
    ```bash
    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10/amd64/linux-headers-5.10.0-051000_5.10.0-051000.202012132330_all.deb
    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10/amd64/linux-image-unsigned-5.10.0-051000-generic_5.10.0-051000.202012132330_amd64.deb
    wget https://kernel.ubuntu.com/~kernel-ppa/mainline/v5.10/amd64/linux-modules-5.10.0-051000-generic_5.10.0-051000.202012132330_amd64.deb
    ```
2. 执行安装
		`dpkg -i *deb`
    更新`grub`:
    `update-grub`
    重启：
    `/sbin/reboot`
3. 删除旧内核
		首先查看内核版本是否正确：
    `uname -r`
    查看机器中所有内核：
    `dpkg --get-selections |grep linux-`
    删除旧版内核：
    `apt remove linux-image-4.19.0-18-amd64`
    再次查看内核：
    `dpkg --get-selections |grep linux-`
    清理残留：
    `dpkg -P linux-image-4.19.0-18-amd64`
## 三、开启BBR

1. 添加bbr配置：
    ```bash
    echo "net.core.default_qdisc=fq" >> /etc/sysctl.conf
    echo "net.ipv4.tcp_congestion_control=bbr" >> /etc/sysctl.conf
    ```

2. 重载sysctl:
		`sysctl -p`

3. 检查bbr运行状态：
		`lsmod | grep bbr`

出现bbr字样，也就说明开启成功了。

> 引用：https://111111.online/posts/kernel_bbr.html