---
title: WSL 指南
description: 
published: true
date: 2021-11-18T09:55:11.901Z
tags: wsl
editor: markdown
dateCreated: 2021-11-18T00:30:28.965Z
---

# WSL 指南

## 一、 安装WSL

1. 以管理员身份打开`Powershell`,输入`wsl --install`进行安装。
> 官方文档：https://docs.microsoft.com/en-us/windows/wsl/install

## 二、 配置环境



## 坑

1. `windows terminal`打开`wsl`的目录问题：
解决办法： 打开`wsl`执行`cd ~ && echo 'cd ~' >> .bashrc`
> 引用：https://github.com/microsoft/terminal/issues/2743#issuecomment-808872556