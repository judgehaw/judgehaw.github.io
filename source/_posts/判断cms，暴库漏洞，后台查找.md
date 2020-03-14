---
title: cms，暴库，后台查找
date: 2018-10-25 09:23:00
copyright: true
tags: 渗透
categories: "渗透" 
---

------

### 判断cms类型

- 判断目标
- 脚本语言
- 操作系统
- 搭建平台
- cms厂商
- 使用工具:
> wvs,wwwscan

------
<!-- more -->

- 网页自动跳转代码：
>  `<meta http-equiv="refresh" content="5,url=(跳转链接)"/>`

------

### 暴库

1. 暴库绕过防下载
- 假设已经通过工具知道数据库的路径，如果知道数据库目录的名称，名称前有防下载的#，则可以通过%23data.mdb进行下载，
> 注：%23是#的16进制编码，%20表示空格

2. 高级语法暴库

- inurl:./..admin../..add..
- inurl:./..admin../..del..
- inurl:/.asp<id=<%<%<%
> Access数据库查看器：破障

------

### 后台查找
1.默认后台:
> admin,admin/login.asp,manage,login.asp等

2.查看网页的链接：
> 一般来说，网站的主页有管理登陆类似的东西，一般在网站最下面批号中，有可能被管理员删掉。

3.查看网站图片的属性:
> 一些网站图片会保存在网站的管理员目录下

4.查看网站使用的管理系统，从而确定后台:
> 例如：织梦

5.用工具扫描目录:
> 如:wwwscan,intellitamper,御剑

6.rebots.txt的帮助：
> rebots.txt文件告诉蜘蛛程序在服务器上什么样的文件可以被查看

7.GoogleHacker
> intitile:后台管理
特定网站：site:xx.com 后台管理 ，site : xx.com  后台登陆，site : xx.com 管理员登陆

8.查看网站使用的编辑器是否有默认后台，密码
> 网站管理员密码猜解工具：hydra，PKAV HTTP Fuzzer，Discuz批量用户密码暴力破解器

------

### 漏洞利用

php版本漏洞：
> php版本信息在百度搜索版本漏洞

