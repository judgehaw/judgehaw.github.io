---
title: Linux系统账户安全
copyright: true
date: 2020-01-19 09:46:32
tags: 安全
categories: “安全”
---

------

**本文将从六个方面来谈Linux系统账户安全，希望能对大家有所帮助。**

------

<!-- more -->

# 设置系统账户有效期

## chage 命令
1. chage -l 账户名称  （查看账户信息）

2. chage -E 时间 账户名称  （修改账户有效期时间）
    例：  

```
  # chage -E 2020-06-18 zhangsan
  # chage -l zhangsan
```

## 系统配置文件定义账户默认有效期

### 配置文件
- /etc/login.dgfs

```
PASS_MAS_DAYS    99999 （密码的最长有效期）
PASS_MIN_DAYS    0 （密码的最短有效期）
PASS_MIN_LEN    5 （密码的最短长度）
PASS_WARN_AGE   7 （密码过期前多少天提示警告信息）
UID_MIN    1000 （新用户创建时的UID最小值）
UID_MAX    3000 （新用户创建UID最大值）
```

> UID：UID是系统在创建账户时自动生成的用户标识号，与账户名唯一对应，可以在系统/etc/passwd文件中查看，文件中每个":"分割参数对应为用户名、口令占位符、UID、GID(用户组标识号)、描述、主目录、指定的登录shell。不需要登录shell的用户可以设置其登录shell为/usr/sbin/nologin，修改UID一般用 usermod -u命令。

------

# 锁定用户账户，使其无法登录

## 命令

```
1. passwd -l zhangsan  锁定zhangsan用户
2. passwd -S zhangsan  查看账户状态
3. passwd -u zhangsan  解锁用户账户
```

------

# 修改tty登录的提示信息，隐藏系统版本

## 配置文件

- /etc/issue  保存系统登录前提示信息的文件，修改后重启生效。
- 
> 可设置的参数：
/l  显示第几个终端机接口
/m  显示硬件的等级
/n  显示主机的网络名称
/o  显示 domain name
/r  显示操作系统的版本
/t  显示本地端时间
/s  显示操作系统的名称
/v  显示操作系统的版本

------

# 锁定文件

```
1. chattr  +i  文件名    锁定文件，使其无法修改和删除  -i 解锁
2. chattr  +a  文件名  锁定后只能在文件后追加内容，常用于日志文件  -a 解锁
3. lsattr  文件名  查看文件的特殊属性
```

------

# sudo 自定义分配管理权限，用户权限分离

## 配置文件
- /etc/sudoers

```
sudo -l  查看当前用户的sudo权限
```

> 例：授权zhangsan以root身份执行systemctl命令，使其能进行服务的启动、关闭等操作。
zhangsan  ALL=(ALL)  /usr/bin/systemctl 

> 例：允许用户lisi通过sudo的方式添加、删除、修改除root以外的用户账号权限。
> lisi  ALL=(ALL)  /usr/bin/passwd,!/usr/bin/passwd root,/usr/sbin/user\*,!/usr/sbin/user\*  *  root

> 例：允许admin组的所有成员以root权限执行所有命令。
> %admin  ALL=(ALL)  ALL

## 为sudo机制启用日志记录，以便跟踪sudo执行的操作

```
 # visudo 
Defaults logfile = "/var/log/sudo" 
```

------

# 提高ssh服务安全

## 调整sshd服务配置

### 配置文件
- /etc/ssh/sshd_config

#### 常用配置

```
Port 2222  改用非标准端口
Protocol 2  启用ssh2协议版本
PermitRootLogin no  禁止root用户登录
PermitEmptyPasswords no  禁止密码为空的用户登录
LoginGraceTime 2m  登录限时2m
MaxAuthTries  3  每次登录的最大认证次数
```
**修改后重启sshd服务**

## ssh黑名单和白名单

配置文件`/etc/ssh/sshd_config`增加配置

```
AllowUsers zhangsan lisi@192.168.40.0/24 允许zhangsan用户在任意网段登录，允许lisi用户在192.168.40.0网段登录。
DenyUsers wangwu  黑名单，不允许wangwu用户登录ssh
```

## ssh密钥免密码登录

1. 生成密钥：`ssh-keygen`
2. 发送公钥给指定IP终端：`ssh-copy-id  root@192.168.40.2`，可以通过`-i`选项来指定密钥，`-i /root/.ssh/id_rsa`
3. sshd配置文件禁止密码登录：
```
PasswordAuthentication  no  将默认的yes改成no
```
**先简单介绍这些，希望大家能对自己系统的账户安全进行重视，感谢！鞠躬！**