---
title: Git的基本操作
copyright: true
date: 2020-01-16 14:58:01
tags: git
categories: "git"
---
------

## git的基本框架
------
<!-- more -->

### 本地仓库

#### 这里总结的基本是使用git bash的命令行进行操作，第三方软件进行点击按钮的操作相对简单。

&ensp; &ensp; 我们在安装好git之后，先在本地新建一个工作目录，目录的位置可以自己来定，也可以在桌面右键，点击Git Bash Here，使用cd命令进入自己想要创建工作目录的路径，然后使用mkdir [目录名]在当前路径创建工作目录。

&ensp; &ensp; 在我们创建好工作目录之后，在Git Bash中使用命令`git init`在当前目录生成本地git版本库，hooks：存放一些shell脚本，Info：exclude：存放仓库的一些信息，logs：保存所有更新的引用记录，objects：存放所有的git对象，config：git仓库的配置文件，description：仓库的描述信息，主要给gitweb等git托管系统使用，index：暂存区（stage）一个二进制文件，HEAD：映射到ref引用，能够找到下一次commit的前一次哈希值，ORIG_HEAD:HEAD指针的前一个状态..等等。

&ensp; &ensp; 本地仓库分区三个区域：工作区、暂存区、仓库。
&ensp; &ensp; 这三个区域分别对应不同的状态，这个状态我们可以在Git Bash中使用命令`git status`来查看。


> 工作区：工作区是我们在编辑代码文档等的区域，在这个区域我们可以使用`git status`查看到当前修改过或新增的文件都为红色字体，我们可以通过其显示的状态来确定当前文件有没有被修改“Untracked files”.


> 暂存区：暂存区是我们可以将编辑好的文档等暂时保存的区域，将工作区的文档等提交暂存区的命令`git add /路径/文件名`，这时候我们使用`git status`可以看到绿色字体，里面是我们创建并提交到暂存区的文件。


> 仓库：本地仓库是我们保存本地所有经过上传的文件，这些文件会默认保存在master分支，master分支是git生成版本库`git init`的时候默认自动创建的分支，我们将暂存区的文档等提交到本地仓库的命令是`git commit -m "更新说明"`，提交到仓库之后我们在使用`git status`就会提示我们`nothing to commit, working tree clean`，表明我们将所有新文件都提交到了本地仓库，这时候的暂存区是干净的。

------

### 分支

&ensp; &ensp; 我们可以使用`git branch -a`来查看当前的所有分支包括远端仓库分支，绿色前面带*的是我们当前分支。

&ensp; &ensp; 分支的作用：几乎所有的版本控制系统都以某种形式支持分支。 使用分支意味着你可以把你的工作从开发主线上分离开来，以免影响开发主线。 在很多版本控制系统中，这是一个略微低效的过程——常常需要完全创建一个源代码目录的副本。对于大项目来说，这样的过程会耗费很多时间。

#### 如何连接GitHub仓库分支

&ensp; &ensp; 怎样将GitHub跟本机使用ssh建立连接可以看前面搭建博客第一篇的内容，里面有介绍。

&ensp; &ensp; 在我们跟GitHub项目建立连接之后，使用`git remote -v`来查看远程项目地址，之后 我们使用`git pull`可以直接将GitHub项目仓库中的所有内容都拉取到本地并跟当前合并，这个命令跟`git fetch`结果相似，不过有所不同，不同之处[点击这里](https://www.cnblogs.com/runnerjack/p/9342362.html)

&ensp; &ensp; 当我们拉取结束之后，我们就可以使用`git branch -a`查看到我们本地和远端的所有分支了，红颜色的代表远端分支，`git branch -r`只查看远程分支。

&ensp; &ensp; 切换分支`git checkout origin/分支名称`，切换分支之后我们命令行右上角会看到当前分支的名称，或者使用`git branch -a`来查看当前分支。

------

### Git使用的基本流程

&ensp; &ensp; 首先我们是在我们本地的工作区来新增或修改文档代码等，然后通过`git add .`将我们已经处理好的文本添加在本地的缓存区，这个时候`git status`可以看到绿色的字体为我们修改过或新增的文本。
&ensp; &ensp; 在我们将所有修改文本全部加入缓存区之后，我们需要将其提交至本地的Git仓库，使用`git commit -m "情况说明"`这里的说明是非常重要的，它可以在我们回退之前提交的Git版本的时候查看到。
&ensp; &ensp; 在我们将文本提交到本地仓库之后，我们通常会需要将其上传至远端GitHub的项目中，这时候我们使用命令`git push origin master(master是我们需要上传的分支)`这里分支的概念就尤为重要了，`git push origin master:hexo`这里的“:”是将我们当前master分支的内容上传合并至hexo分支中。
这样我们在本地写的文本便会被我们存到GitHub中，就可以实现多终端的版本控制了。

------

### 多终端同步

&ensp; &ensp; 多终端同步在我们平时的版本控制中是常见的需求，首先我们需要在新的终端中安装好Git环境，这个环境需要的软件和安装百度也是有很多，大体都差不多。
&ensp; &ensp; 在我们搭建好环境后，我们需要在GitHub中配置好我们当前电脑的ssh密钥，不配置的话我们便会需要在上传的时候输入自己的用户名及密码，比较麻烦，绑定ssh密钥也比较简单，我们首先生成自己的本地密钥，然后在GitHub的Settings里有ssh项，我们创建新的ssh密钥，将我们刚才生成的公钥放进其Key中，点击添加就OK。
&ensp; &ensp; 绑定好ssh之后我们先检查我们本地是否连接上远端GitHub，使用命令`ssh git@github.com`回显出现` You've successfully authenticated`就说明连接成功。
&ensp; &ensp; 这时候我们只需要将我们远端GitHub中的内容拉取到本地就可以了，使用`git pull`命令，需要我们等待一会，如果出错一定是我们之前有哪一步没有正确完成，我们根据报错信息来解决就好。
&ensp; &ensp; 拉取结束之后，就可以在本地的工作区展开工作了，当我们需要上传的时候可以查看我上面写到的步骤，如果我们将.git本地版本库删掉了，我们便需要重新拉取一下，将我们GitHub中的分支等信息得到，不然我们在`git init`之后我们本地只有一个master分支，我们在上传的时候是会报错的。

------

## Git的常用命令


```
创建版本库：git init
添加文件到缓存区：git add 文件路径
添加当前工作区全部文件到缓存区：git add .
添加到仓库：git commit -m "情况说明"
从暂存区回退上个版本：git checkout -- file
删除：git rm file
查看指定文件：git cat file
比对工作区与版本库：git diff
查看日志：git log
操作历史：git reflog
版本回退：git reset --hard commit-id/HEAD-id
创建并指向分支：git checkout -b <branch>
切换分支：git checkout <branch>
本地建立远程库对应分支：git checkout -b branch origin/branch
查看分支信息：git branch -a/v
删除分支：git branch -d <branch>
检测ssh连接：ssh -T git@github.com
添加远程版本库：git remote add origin git@github.com:路径/版本库.git
克隆远程分支库：git clone 远程仓库路径
克隆指定分支：git clone -b 分支名 远程仓库路径
从远程库拉取最新提交：git pull
关联并推送本地版本库到远程库：git push -u origin master
查看远程仓库状态：git remote show origin
更新远程信息：git remote update
```
------

**希望这篇文章对大家理解Git有所帮助，鞠躬！**

------

参考链接：
[Git常用命令总结及一些问题思考](https://blog.csdn.net/tianbo_zhang/article/details/78981905)
[Hexo搭建博客并实现多终端同步管理](https://www.imooc.com/article/9707?block_id=tuijian_wz)
[【教程】学会Git玩转Github【全】](https://www.bilibili.com/video/av10475153?p=7)
[git的工作区域](https://blog.csdn.net/weixin_41900719/article/details/97237256)