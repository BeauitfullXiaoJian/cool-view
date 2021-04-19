---
title: Git笔记
date: 2019-12-09 10:15:22
tags: ['Note']
categories: Git
---

Git 是一个开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。Git 是 Linus Torvalds 为了帮助管理 Linux 内核开发而开发的一个开放源码的版本控制软件。Git 与常用的版本控制工具 CVS, Subversion 等不同，它采用了分布式版本库的方式，不必服务器端软件支持。[点击查看菜鸟教程链接](https://www.runoob.com/git/git-tutorial.html)
<!-- more -->

# 常用场景

## 项目初始化

```bash
# 本地文件夹初始化,并提交到远程仓库

git init

git add .

git commit -m "first commit"

git remote add origin https://github.com/xxxx/xxxx.git (添加远程仓库地址)

git push -u origin master (推送到指定仓库的master上)
```

## 分支操作
1. 从当前分支检出一个新的分支
`git branch branchName`

2. 查看当前所有分支
`git branch`

3. 切换到指定分支
`git checkout branchName`

4. 合并分支到当前分支
`git merge branchName`（比如先切换当前分支为为master主干，再git merge dev将dev分支合并到master）

5. 删除指定分支
`git branch -d branchName`

## 提交撤回

1. 删除上一次push操作（删除上次提交后本地和远程仓库的数据都将删除，所以删除上次提交前，记得备份备份备份数据）

```bash
git reset --hard HEAD^
git push origin master -f
```

2. 重新提交（上一次commit漏添加文件，commit消息写错了）

```bash
git commit -m "我写错了提交消息"

git add 我漏掉的文件.avi

#(输入这行指令会要求你重新编辑上一次commit的内容，可以修改提交信息，git add 也会和上一提交次合并)
git commit --amend 
```

3. 撤销错误的 git add 操作

```bash
git add 一个不该被添加的文件.jpg

git reset HEAD 一个不该被添加的文件.jpg (执行后这个文件将从暂存区域移除)
```

## 打上标签(TAG)

1. 列出已经用的Tag
`git tag`

2. 筛选
`git tag -l "v1.*"`

3. 查看Tag详情
`git show Tag名字`

4. 新建Tag
`git tag -a Tag名字 -m "备注信息"`

5. 给指定提交打上Tag
`git tag -a Tag名字 提交记录 -m "备注信息"`
`git tag -a v1.2 9fceb02 -m "这是一个备注信息"`

6. 推送到远程仓库
`git push origin Tag名字`

7. 切换到指定Tag
`git checkout Tag名字`

8. 从切换的Tag检出一个新分支
`git checkout -b [new-branch-name]`

9. 删除分支

```bash
# 本地删除
git tag -d Tag名称
# 远程仓库删除
git push origin :refs/tags/Tag名称
```

## 将当前修改提交到指定分支
```bash
# 创建新分支
git branch dev

# 将工作区恢复到上次提交的内容，同时备份本地所做的修改
git stash

# 切换分支
git checkout dev

# 从 git 栈中获取到最近一次 stash 的内容，之后会删除栈中对应的 stash
git stash pop

# 添加所有（已修改）文件
git add .

# 添加到本地仓库
git commit -m "xxxx"

# 获取
git pull origin 远程名称

# 推送
git push origin 远程名称
```

# 配置文件

1. 打开全局配置文件
`git config --edit --global （去掉--global为当前项目的配置文件）`

2. 设置指定参数

```bash
git config --global user.email "you@example.com"
git config --global user.name "Your Name"
```

# SSH KEY
ssh-keygen -t rsa -C "example@mail.com"