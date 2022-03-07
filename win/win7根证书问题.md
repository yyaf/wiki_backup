---
title: Win7根证书问题
description: 
published: true
date: 2022-03-07T03:04:59.889Z
tags: 
editor: markdown
dateCreated: 2022-03-07T03:04:59.889Z
---

# Win7 根证书问题
windows没有足够信息、不能验证该证书、windows 7 证书无效、Google Chrome 您的链接不是私密连接，等证书相关问题解决办法。
 
1. 调整系统时间。
	“更改时间和日期设置”之后可以手动设置正确时间或者和互联网时间同步。

2. 升级根证书
	新装的win7系统，某些网站总是提示证书错误或者不受信任。根本原因是win7自带的IE8很多网站已经不支持，证书不再支持。所以某些网站可以，某些网站不行。
	 - 升级到IE11以上 如果找不到升级办法可以尝试第二种
   - 第二种方法是下载根证书，链接如下
		https://www.wosign.com/download/winroot.htm
		WoSign 提醒您更新您的Windows 根证书，请检查发行者签名为Microsoft Corporation，点击“安装”或“是”自动完成!
		如果您的电脑已经设置Microsoft为信任的发行者，则不会出现需要您确认的页面而自动完成更新！
    
3. 重启


> 来源：https://blog.csdn.net/dreamtheworld1/article/details/105038076