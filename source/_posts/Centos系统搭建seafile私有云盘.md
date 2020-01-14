---
title: Centos系统搭建seafile私有云盘
copyright: true
date: 2020-01-14 21:19:16
tags: seafile
categories: "运维"
---

------
## 本文主要说明我在搭建seafile服务器中遇到以及填的坑，seafile介绍等请查看官网
[seafile官网](https://cloud.seafile.com/published/seafile-manual-cn/home.md)

------
<!-- more -->


## 用docker部署seafile开源版
### 服务器系统安装及yum源配置这里就不过多解释了，从docker安装开始，docker介绍等信息请自行Google,下面附B站某docker的教学视频：
[最新docker构建微服务框架 零基础入门](https://www.bilibili.com/video/av23053656?p=1)

> 安装docker：

```
# yum -y install docker docker-compose
```
> 启动服务：

```
# systemctl start docker
```
-----
###  下载docker-compose.yml可执行文件并运行


[docker-compose.yml](https://docs.seafile.com/d/cb1d3f97106847abbf31/files/?p=/docker/docker-compose.yml)

###### 这里说一下这个文件的作用，这个是docker的可执行脚本，利用这个文件可以进行开机自启动，容器启动配置等操作。
###### 这里也可以用pull来拉取docker hub上的镜像，不过建议用yml脚本，因为pull拉取的镜像只有seafile本身，而这个程序是需要数据库的。

> 启动脚本:

```
# docker-compose up -d
```
###### 脚本启动之后docker会自动从docker hub上下载所需的镜像，这里只要等在结束即可。
-----
## 填坑阶段
###  下载完成后查看下载到的容器镜像

```
# docker images
```
-----
###  脚本运行结束后查看容器运行状态：

```
# docker ps 
# docker ps -a
```
-----
###  查看正在运行和后台的所有容器。这时候应该是有3个容器的，分别是：

> seafileltd/seafile-mc:latest

> memcached:1.5.6

> mariadb:10.1

####  如果查看到有没有运行的容器可能是由系统内存不足引起的，系统内存需在2G以上，将正在运行的容器停止后重新运行脚本并查看状态：

```
# docker stop {容器ID} 停止正在运行的容器
# docker rm {容器ID} 删除容器
# docker-compose up -d 重新运行脚本
# doker ps 查看正在运行的容器
# free -m  查看内存
```
####  启动时报错端口占用问题：

> 容器启动时需要占用系统80端口，这个端口可以在脚本中进行改动。

```
service：
    ...
    seafile:
        ports:
            - "80:80"
    ...
```
> 如果需要80端口的话，查看80端口运行的服务，并停止服务即可。

```
# ss -untlp | grep :80 查看端口对应的服务
```
> 如果是nginx服务的话，关闭并取消服务自启动。

```
# systemctl status nginx 查看服务状态
# systemctl stop nginx 关闭服务
# systemctl disable nginx 取消自启动
```
####  启动脚本时报错找不到docker-compose.yml文件问题。
> 这个问题主要是因为当前路径找不到脚本，我们可以进入脚本目录或将脚本移动到家目录执行。

```
# pwd 查看当前目录
# find / -name docker-compose.yml 在系统根查找脚本文件
# mv docker-compose.yml ~ 移动脚本文件至家目录
```
> 移动至家目录之后就可以再次执行启动脚本了。
####  启动过程中报错：
> 这种情况有可能是容器数据目录跟容器不匹配导致容器无法启动。
> 如果我们是第一次搭建的话，可以先查看数据目录的路径，之后删除整个目录。

```
# vim docker-compose.yml
service:
    db:
        volumes:
        - /opt/seafile-mysql/db:/var/lib/mysql
    seafile:
        volumes:
        - /opt/seafile-data:/shared
```
> 删除之后我们重新启动脚本，系统会自动重新生成数据目录。
-----
### 此时我们的seafile基本是搭建完成了

####  在浏览器输入localhost即可进入seafile登录界面。

> 在启动脚本中查看登录用户名及密码登录seafile

```
# vim docker-compose.yml
service:
    seafile:
        environment:
            - SEAFILE_ADMIN_EMAIL=admin@qq.com
            - SEAFILE_ADMIN_PASSWORD=123456
```
> 脚本中的管理员用户及密码可以自己修改。
####  成功登录后，我们需要页面设置里面进行重新配置。

```
点击右上角头像-->系统管理-->设置
```
> URL栏修改自己的服务器IP地址，其他设置可以自己根据需要来进行。

## 虚拟机搭建服务器后局域网访问
> 我们如果是用Linux虚拟机搭建的seafile服务器，可以通过VMware网卡net端口转发来将虚拟机的80端口映射到真机的80或其他端口。

```
操作流程：
VMware菜单栏编辑-->虚拟网络编辑器-->更改设置-->选择VMnet8网卡-->net设置-->端口转发下面添加-->填写自己需映射到主机的端口，类型TCP，虚拟机IP及虚拟机端口地址，备注seafile。
```
> 设置完成后就可进行局域网访问测试了。

### 文章最后附几个自己在搭建过程中参考的教程链接，希望能对大家有所帮助。

[知乎 利用Seafile搭建私有文件同步云盘](https://zhuanlan.zhihu.com/p/55655106)

[简书 基于Docker+Seafile搭建私人云存储](https://www.jianshu.com/p/35762b77950f)

[官网 用 Docker 部署 Seafile 服务](https://cloud.seafile.com/published/seafile-manual-cn/docker/%E7%94%A8Docker%E9%83%A8%E7%BD%B2Seafile.md)

### 祝各位大佬在学习及工作中万事顺心，鞠躬！