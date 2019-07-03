---
title: Git笔记和hexo笔记
date: 2019-07-03 17:01:25
tags: Git
desc: 我常用的命令
categories: 
	- Git
---


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
<!-- 阅读更多 -->
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