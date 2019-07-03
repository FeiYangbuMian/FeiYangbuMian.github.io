---
title: Git笔记和hexo笔记
date: 2019-07-03 17:01:25
tags: Git
desc: 我常用的命令
categories: 
	- Git
---

git常用命令只有几个，但由于不常使用，基本记不住，于是便总结为一个笔记，用时就打开复制一下。
之前我只是将git当代码仓库使用。昨天制作了基于hexo的git博客，今天有换了电脑。于是将所用命令再次整理下。

<!-- more -->
## 目前我使用的命令步骤
1. git add .
2. git commit -m "some descrption"
3. git push origin hexo
4. hexo g -d

来源于最底下链接内容：
```
迁移工作已完成，在两台电脑之间的同步操作如下：

git pull从远程hexo分支拉取最新的环境文件到本地，可以理解为svn的更新操作。比如在公司写了博客，回家在电脑上也要写需要先执行这一步操作。
文章写完，要发布时，需要先提交环境文件，再发布文章。按以下顺序执行命令：git add .、git commit -m "some descrption"、git push origin hexo、hexo g -d。

作者：nikolausliu
链接：https://www.jianshu.com/p/fceaf373d797/

```


## git使用

拉项目到本地
```
1. 新建文件夹
2. git clone 项目复制的地址
```

初次创建仓库： 
```
1. 在网上创建一个仓库，git clone 仓库地址
2. 把项目复制进仓库名一样的文件夹内，cd 仓库名文件夹
3. 将文件加入暂存区，git add . 
4. 暂存区内容提交到仓库，git commit -m "注释内容"
5. 把本地仓库的项目推送到github上，git push -u origin master
    // 第一次上传成功后可以去掉-u
```

仓库里已有文件再提交：
```
1. cd 仓库名文件夹
2. 将文件加入暂存区，git add .     
3. 暂存区内容提交到仓库，git commit -m "注释内容"
4. 获取远程库与本地同步合并，git pull --rebase origin master
6. 把本地库内容推送到远程库上，git push -u origin master
	 //第一次上传成功后可以去掉-u
```
## hexo使用

### 新建文件

```
hexo new 文件名   
```
### 编写完后

```
1. hexo clean 清除缓存文件
2. hexo g  生成文件
3. hexo d 推送到远端github仓库
```


由于换成台式电脑，hexo迁移遇到一些问题，根据此链接慢慢解决了：[hexo博客同步管理及迁移](https://www.jianshu.com/p/fceaf373d797/)