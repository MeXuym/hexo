---
title: Hexo多端更新和备份
date: 2017-01-4 11:41:28
categories: "git"
tags: ["git","Hexo"]
---


偶然回老家一趟，想在hexo上写点老家的见闻。
上去github想把hexo博客的仓库clong下来，发现原来每次执行hexo deploy只是把生成的页面文件布上去而已。其余的如hexo源文件和主题文件并没有放到github。就想到要怎么样可以在换电脑的时候也可以更新hexo,也起到备份的作用，尝试了两种办法。

## 新开一个仓库放源文件
### 本地hexo关联远程仓库
这个办法几乎没有花什么脑力就想出来了，也是最简单的办法。
在github上面新开一个仓库，关联本地的hexo。
cd到hexo目录（默认你已经弄好hexo写过东西了）
<!--more-->
```
git init
git remote add origin ...
git pull origin master 
git add .
git commit -m ""
git push origin master
```
这样一来新建的仓库上就备份了你的hexo。

### 修改hexo
在你写hexo博客的时候，还是和以前一样流程。
修改文件然后：

```
hexo clean
hexo g
hexo deploy
```
访问 mexuym.github.io，OK正常。然后：

```
git commit -m ""
git push orgin master
```
也备份到新仓库了。

### 换新电脑
换到我的台式机，配置git,关联git...
修改文章，发布，备份，一切正常。

### 缺点
新建一个仓库来备份源文件，的确达到目的了，但是用了两个仓库来管理hexo，怪怪的，于是又另外尝试了一下。


## 管理不同分支来达到目的
### 搭建流程
创建仓库mexuym.github.io
创建两个分支：master 与 hexo
设置hexo为默认分支
git clone 拷贝仓库
在本地mexuym.github.io文件夹下通过Git bash依次执行（此时当前分支应显示为hexo）

```
npm install hexo
hexo init
npm install
npm install hexo-deployer-git
```

修改_config.yml中的deploy参数，分支应为master
依次执行下面的命令提交网站相关的文件
```
git add .
git commit -m “…”
git push origin hexo
```
执行hexo generate -d生成网站并部署到GitHub上。
这样一来，在GitHub上的mexuym.github.io仓库就有两个分支，一个hexo分支用来存放网站的原始文件，一个master分支用来存放生成的静态网页

### 管理流程
#### 日常修改
依次执行
```
git add .
git commit -m “…”
git push origin hexo
//将改动推送到GitHub（此时当前分支应为hexo）
hexo generate -d
//然后才执行hexo generate -d发布网站到master分支上
```
#### 重装系统
git clone 拷贝仓库（默认分支为hexo）
在本地新拷贝的mexuym.github.io文件夹下通过Git bash依次执行下列指令：
```
npm install hexo
npm install
npm install hexo-deployer-git //（记得，不需要hexo init这条指令）。
```

参考链接：[GitHub Pages + Hexo搭建博客](http://crazymilk.github.io/2015/12/28/GitHub-Pages-Hexo搭建博客/#more)
