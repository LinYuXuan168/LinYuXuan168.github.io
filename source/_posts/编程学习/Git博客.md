---
title: Git入门学习
date: 2023-12-16
description: Git的学习和日常使用推送
tags:
  - Git学习
categories:
  - 技术学习
music:
  server: netease
  type: playlist
  id: 8109052701
author: 林宇轩
---

本篇文章主要简述 Git 的基本介绍和常用的命令，以及在实际的应用

<!-- more --> 

## Git 介绍

- Git 是一个免费的开源的分布式版本控制系统，用于进行项目的管理

---

## Git 的文件状态

- Git 的文件状态分为三种：**已提交**，**已修改**，**已暂存**，文件流转在**本地工作目录**，**暂存区**，**本地仓库**，**远程仓库。
- Git 文件时，先要把文件**add**到暂存区，然后**commit**到本地的仓库，再从本地仓库**push**到远程的仓库。

---

## Git 的日常使用命令

- Git 的日常管理是比较简单的，记住一下几个命令即可

```shell
# 默认仓库已经是初始化好，创建好的
--- 
git clone # 远程克隆远端的仓库
git pull  # 拉取远端仓库的内容，同步本地和仓库
--- 
# 在本地的仓库进行修改之后
git status # 查看当前修改文件的状态
git add . # 将修改的文件添加到暂存区
git commit -m "add your instruction" # 将暂存区的文件添加到本地仓库
--- 
git push # 将本地仓库的文件提交到远程仓库
```

---

## Git 其他常用命令

- 以下是一些常用的 Git 的命令，只有在特定的时候或场景需要被用到

```shell
# 初始化Git代码库
git init 
# 配置Git
git config [--global] user.name "name"
git config [--global] user.email "email address"
# 添加文件
git add "指定文件"
# 提交
git commit -amend -m [message] # 使用新的commit代替上一次的提交，（常用来改写上一次commit的message
# 查询
git branch # 列出本地分支
git branch -r # 列出远程分支
git log # 查看当前分支的版本历史
git log --stat # 查看commit历史
git diff # 显示暂存区和工作区的差异
# 拉回
git pull [remote][branch] # 取回远程仓库的变化，与本地分支合并
# 强制拉回，远端覆盖本地
git fetch --all
git reset --hard origin/{branch_name}
# 强制推送
git push -f origin branch_name
# 回滚
git checkout [file] # 恢复暂存区的指定文件到工作区
git checkout [commit][file] # 恢复某个commit的文件
git reset --hard [commit id] # 回滚到某一次提交的版本
git reser --hard HEAD^ # 回退到上一个版本
# 使用revert是撤销一次提交，所以后面的commit id是你需要回滚到的版本的前一次提交,一般用来回退公共远程分支
git revert HEAD # 撤销最近一次提交 
git revert HEAD~1 # 撤销上上次的提交，注意：数字从0开始
git revert 0ffaacc # 撤销0ffaacc这次提交
```

---
