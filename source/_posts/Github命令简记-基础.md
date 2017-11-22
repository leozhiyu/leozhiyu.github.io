---
title: Github命令简记-基础
copyright: true
date: 2017-08-31 21:21:32
tags:
    - github
categories:
    - 工具
    - github
---

刚刚学习github， 本文记录最基本的github命令， 网上教程一大把， 我简单将命令记录在此， 方便查找。

<!--more-->

### 本地环境操作

- `git` ------------------------------------- 查看最常用的Git命令

- `git status`  ----------------------------- 查看当前仓库的状态

- `git init`  -------------------------------- 初始化 git 仓库

- `git add  [ name ]` --------------------- 将代码加入等待提交的缓存中（防止误提交）

- `git rm –-cached [ name ]`  ---------- 从缓存中移除

- `git commit -m [ message ] ` --------- 提交附带提交信息

- `git log`  ---------------------------------- 查看commit记录

- `git branch` ------------------------------- 查看当前分支情况

- `git branch [ name ]` ------------------- 创建新分支

- `git checkout [ name ]` ---------------- 切换分支

- `git checkout -b [ name ]` ------------ 创建并切换分支

- `git merge [ name ]` ---------------------合并分支到主分支 

- `git branch -d [ name ]`---------------- 删除分支

- `git branch -D [ name ]` --------------- 强制删除分支

- `git tag` -------------------------------------查看版本记录

- `git tag [ version ]`---------------------新建版本信息

- `git checkout [ version ] `--------------切换到对应的版本状态


### 本地与远程仓库协作

- `ssh -T git@github.com` --------------------------查看SSH key 是否添加成功

- `git remote add origin [ url ]` ---------------建立本地目录与远程仓库的关联 ( 相当于给远程 url 取了别名 origin )

- `git remote remove origin` ----------------------取消本地目录与远程仓库的关联 

- `git remote -v` --------------------------------------查看当前项目有哪些远程仓库

- `git push origin master` --------------------------将本地代码提交到远程仓库 ( 需要先解决冲突 )

- `git push -f origin` -------------------------- 强制提交( 覆盖远程代码 )

- `git pull origin master` ------------------------- 更新远程仓库代码到本地

- `git clone [ url ]` -------------------------------- Clone 项目

- `git checkout [ version ]` ------------------------切换到对应的版本状态

- `git config --global user.name 「 username 」`------- 全局设置用户名 ( 去除global 就是为某一个项目进行设置 )

- `git config --global user.email 「 email 」`----------全局设置邮箱 ( 去除global 就是为某一个项目进行设置 )

