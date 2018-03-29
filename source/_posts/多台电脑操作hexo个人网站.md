---
title: 多台电脑操作hexo个人网站
date: 2018-03-28 01:12:54
categories: hexo
tags: [hexo]
---
## 前言
> 最近换了个电脑，于是出现在新电脑上如何更新hexo个人网站的问题，网上各种方法，个人觉得还是有些不明白的地方，踩坑无数。最后终于成功，所以特此记下，也希望给其他人能有一些帮助。文章中用‘旧电脑’指代原来已经搭建好hexo的电脑，‘新电脑’指代即将要搭建hexo环境的电脑。

<!--more-->

## 旧电脑上的操作
### 准备工作
首先确保自己已经使用hexo在github搭建好了自己的个人博客，github仓库中如下图显示：![image](https://upload-images.jianshu.io/upload_images/2859254-e60e6b7e367b36fa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)
### 对username.github.io仓库新建hexo分支，并克隆
在Github的username.github.io（就是hexo网站的仓库）仓库上新建一个xxx（自己取名，我取名为hexo）分支，并切换到该分支，并在该仓库->Settings->Branches->Default   branch中将默认分支设为xxx，save保存。  

新建分支，并选中新建的分支，如下图：

![image](https://upload-images.jianshu.io/upload_images/2859254-01cb597e80e5005b.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

设置新建的xxx分支为默认分支，如下图：

![image](https://upload-images.jianshu.io/upload_images/2859254-67ed8c22531dd66d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

然后将该仓库克隆到本地，进入到本地的username.github.io文件目录。  
进入本地的username.github.io后，在当前目录使用git bash执行git branch命令查看当前所在分支，应为新建的分支xxx。  
### 将本地博客的部署文件拷贝进username.github.io文件目录
先将本地博客的部署文件（就是之前存放hexo项目的目录下的全部文件）全部拷贝进username.github.io文件目录中去。

接下来，进入username.github.io文件目录下，依次执行：
1. **git add .**
2. **git commit -m '新电脑部署'**（引号内容可改）
3. **git push**

即可将博客的hexo部署环境提交到GitHub个人仓库的xxx分支。  

此时部署到github的步骤就已经完成了。master分支和xxx分支各自保存着一个版本，master分支用于保存博客静态资源，提供博客页面供人访问；xxx分支用于备份博客部署文件，供自己修改和更新，两者在一个GitHub仓库内互不冲突，完美！
## 新电脑上的操作
到这一步，你的博客已经可以在其他电脑上进行同步的维护和更新了，方法很简单：
1. 将新电脑生成的ssh key添加到GitHub账户上（生成并添加ssh key的方法自行百度，本文不做详细说明）。
2. 在新电脑上克隆username.github.io仓库的xxx分支到本地，此时本地git仓库处于xxx分支.可以使用git branch查看当前分支。
3. 切换到username.github.io目录，执行npm install(由于仓库有一个.gitignore文件，里面默认是忽略掉node_modules文件夹的，也就是说仓库的hexo分支并没有存储该目录，所以需要install下)
4. 在新电脑上安装hexo命令
```
$ npm install -g hexo-cli
```
## 新旧电脑更新网站的方法
首先注意一点：每次换电脑进行博客更新时，不管上次在其他电脑有没有更新，最好先git pull，防止冲突，这是一个好习惯。
然后，依次执行以下步骤：
1. 编辑、撰写文章或其他博客更新改动，就是你要对博客进行的修改，或新增文章。
2. 依次执行git add .、git commit -m '在新电脑上提交新文章'（引号内容可改）、git push指令，保证xxx分支版本为最新版本。
3. 依次执行hexo g,hexo d指令（执行之后，过几分钟如果网站上的内容没有更新，在hexo g之前，可能需要执行hexo clean），完成后就会发现，最新改动已经更新到master分支了，两个分支互不干扰！

到此，就可以在多台电脑上更新个人网站啦！如有疑问，欢迎留言！
## 参考资料
[利用Hexo在多台电脑上提交和更新博客](https://www.jianshu.com/p/0b1fccce74e0)