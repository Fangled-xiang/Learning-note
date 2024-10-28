# Git概述

​	Java、后端、大数据、	开源！！、分布式版本控制工具，管理代码‘

Git分支：创建、转换、合并、代码冲突、  Idea集成Git（和Linux是同一个开发创始者Linus）

**GitHub：**推送Push、拉取Pull、代码克隆Clone、SSH免密登录

Gitee码云（中国版的GitHub）

GitLab：公司内部用的，局域网服务器



版本控制：记录内容变化、修改记录

集成式Vs分布式：集成需要共用一台中央服务器、看的是同一套代码，致命缺点是单点故障问题

​		分布式具有远程库、代码托管中心、先克隆远程库代码在本地开发，修改好再推送

工作机制：工作区（git add）暂存区（git commit）本地库

​	代码托管工具：基于网络服务的远程代码仓库、远程库（局域网：GitLab，互联网:GitHub

,Gitee）

​		Git编辑器选择Vim编辑，方便，Git的客户端操作起来和Linux差不多

​	**Git的底层**：移动的Head指针

# 常用命令

配置：

git config --global user.name 用户名  配置签名来区分用户（和Git上登录的没有关系）

git config --global user.email 邮箱

**常用命令：**

git init 初始化本地库  （在对应的工程文件中，生成.git的隐藏目录）

git status 查看本地库状态   （暂存等状态）

git add 文件

git rm --cached <file> 删除暂存区

git commit -m  “日志信息/版本号（例1.0.0）”  文件名

git reflog  查看历史记录/ git log（详细）  //(HEAD -> master) 指向当前版本

git reset --hard  版本号 （显示在前面）



红色为未提交的文件



补充：Linux下：ll -a查看可以隐藏文件

​		yy复制，p粘贴



# 分支操作

Git的主打特性：

分支：同时推进多个任务，每个任务有单独分支（增加功能）、分支就不会影响主线（master）任务的运行（**底层也是指针的应用**，两个指针的应用）

​	git branch 分支名

​	git branch -v 查看

​	git checkout 分支名    切换

​	git merge 分支名 	合并  （要合并到哪个分支就要进入哪个分支）



​			文件夹里实际内容显示的是：当前指向的分支/主线的版本

合并冲突：两个分支在同一个文件夹的同一个位置有两套不一样的修改

​		主线修改并提交后，进入分支，此时分支是显示主线没有修改的（或者说是创建时的版本），如果分支也进行修改后提交（不提交无法回归主线），然后回主线合并就有问题。此时查看文件会显示冲突的部分。**进行手动的修改！！！然后提交本地库！！**！（且commit时不带文件名、且只修改合并的分支，回去被合并的分支不改变）



## 团队协作



团队内协作：

管理者推送Push本地库到远程库（代码托管中心），其他开发者克隆下载到本地库，修改好在push远程库（需要权限），管理者再pull修改的代码，并更新本地库代码（完成三个库的同步）



团队外协作：

​	协助者在代码托管中心将其他的远程库，fork到自己的远程库，并clone到自己的本地库，在push到自己的远程库，然后发布pull request到开发者审核，审核通过就可以merge到开发者的远程库，并pull到开发者自己的本地库



pull和clone的区别，一个是自己创建的，一个是完全复制别人的

**注意：修改完都要保存提交本地库、然后才推送远程库**

# GitHub

创建远程仓库别名

​	git remote -v

​	git remote add  别名 https://github.com/Fangled-xiang/Learning-note

​	git remote -v查看别名



推送：

​	git  push  别名 分支（master）

## SSH免密登录：

在c盘用户文件夹，打开git终端，然后添加秘钥

ssh-keygen -t rsa -C 3102467705@qq.com（直接三次回车）

生成.ssh文件













