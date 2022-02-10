---
title: win10 开始菜单无法打开解决办法
description: win10 开始菜单无法打开解决办法。
published: true
date: 2022-02-10T07:40:40.684Z
tags: win10
editor: markdown
dateCreated: 2022-02-10T07:40:40.684Z
---

# win10 开始菜单无法打开解决办法

## 1、使用快捷键`Win+R`弹出运行窗口，在运行窗口输入框中输入`powershell`并点击`确定`。

## 2、在弹出的窗口中复制粘贴命令：`Get-AppxPackage | % { Add-AppxPackage -DisableDevelopmentMode -Register "$($_.InstallLocation)\AppxManifest.xml" -verbose }`，并点击`Enter`确认。

## 3、 使用快捷键`Ctrl+Shift+Esc`会弹出任务管理器窗口，在任务管理器窗口中点击`进程`，然后找到`Windows资源管理器`，选中点击`重新启动`即可。

