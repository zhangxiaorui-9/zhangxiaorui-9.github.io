---
title: git使用笔记（一）
date: 2017-01-16 19:11:09
categories: git
tags: [git]
---
#### 注
文中提到的**工作区**指还没add到暂存区的

**版本库**指已经把暂存区的commit后的
<!--more-->
#### 初始化
初始化一个Git仓库，在当前文件夹，使用git init命令。
#### 添加文件到Git仓库
添加文件到Git仓库，分两步：
1. 使用命令git add filename，注意，可反复多次使用，添加多个文件；
```
$ git add file1.txt
$ git add file2.txt file3.txt
$ git commit -m "add 3 files."
```


2. 使用命令git commit -m ''，引号内为本次提交的说明，完成。

#### 查看当前状态
1. 要随时掌握工作区的状态，使用git status命令。

2. 如果git status告诉你有文件被修改过，用git diff可以查看修改内容。**注意**，使用git diff查看修改内容是在add之前查看。

#### 版本回退
1. 首先，可以用git log查看提交的历史记录。![image](http://ppvq158m9.bkt.clouddn.com/git01.png)

上图可以看出，共提交过两次。在Git中，用HEAD表示当前版本，也就是最新的提交。上一个版本就是HEAD\^，上上一个版本就是HEAD\^\^，当然往上100个版本写100个\^比较容易数不过来，所以写成HEAD~100。
2. 使用git reset --hard HEAD\^命令，回退到上个版本。
3. 回退到上个版本后，又后悔了怎么办？这时我们可以通过commit id来回到你想回到的任何版本。版本号写前面六七位就可以了。如：git reset --hard e8008
4. 找不到commit id怎么办？可以使用git reflog查看你的每一次命令。![image](http://ppvq158m9.bkt.clouddn.com/git03.png)

#### 暂存区
当我们修改或者新增文件，在git add fileName之前，通过git status查看当前的状态

![image](http://ppvq158m9.bkt.clouddn.com/git02.png)

然后通过git add fileName，就可以把修改的文件或者新增的文件放到暂存区。

![image](http://ppvq158m9.bkt.clouddn.com/git04.png)

此时，暂存区有一个修改过的文件和两个新增的文件。

由图中的提示可以看出，我们可以通过git reset HEAD fileName 来使指定文件退出暂存区。

git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit -m''就可以一次性把暂存区的所有修改提交到分支。
#### 撤销修改


1. 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- fileName。

2. 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD fileName，这个文件就回到了工作区，第二步按1操作。

3. 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考**版本回退**，不过前提是没有推送到远程库。

#### 删除文件
##### 第一种情况
当你在本地把文件删除后，这时版本库并没有删除，此时分两种情况：
1. 删除版本库中的，git rm fileName
2. 恢复本地的，git checkout -- fileName。git checkout -- fileName其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

注：版本库是已经提交（commit）的！
##### 第二种情况
当你直接使用git rm fileName命令删除版本库中的文件后，在提交之前，可以参考**撤销修改**部分

![image](http://ppvq158m9.bkt.clouddn.com/git05.png)

#### 参考资料
[廖雪峰的官方网站](https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)