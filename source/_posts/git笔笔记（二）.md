---
title: git使用笔记（二）
date: 2017-01-17 18:44:06
categories: git
tags: [git]
---
## 关联远程仓库

本地仓库与github的远程仓库关联的方式有两种：
<!--more-->
### 第一种情况
第一种情况，我们已经在本地创建了一个git仓库后，又想在GitHub创建一个git仓库，并且让这两个仓库进行远程同步。关联方式如下：
1. 在GitHub上新建一个仓库（Repository），假如仓库命名为learngit;
2. 在本地的git仓库运行命令：
```
$ git remote add origin git@github.com:userName/learngit.git
//userName为你的GitHub账户名
```
3. 把本地库的所有内容推送到远程库上，由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来：
```
$ git push -u origin master
```
4. 以后的推送或者拉取时就可以简化命令：
```
$ git push origin master
```
### 第二种情况
假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。
1. 在GitHub上新建一个仓库（Repository），假如仓库命名为rep;
2. 克隆到本地
```
$ git clone git@github.com:userName/rep.git
//userName为你的GitHub账户名
```
## 分支管理

### 创建与合并分支

注：name为分支名称。
1. 查看当前本地分支：git branch
2. 查看远程分支: git branch -r
3. 查看已有的本地及远程分支：git branch -a
4. 创建分支：git branch name
5. 切换分支：git checkout name
6. 创建+切换分支：git checkout -b name
7. 合并name分支到当前分支：git merge name
8. 删除本地分支：git branch -d name
9. 强行删除本地分支：git branch -D name
10. 删除远程分支：git push origin --delete name