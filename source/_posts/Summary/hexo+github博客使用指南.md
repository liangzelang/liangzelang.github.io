---
title: hexo使用指南
date: 2020-11-11 23:30:21

categories:
- 技术

tags:
- hexo
---

## 写作

创建新的文章
* ```hexo new [layout] <title>```


## 多平台设备同步
经常在不同设备上写博客，所以需要需要在不同的平台设备上同步。主要就是依赖`github`,搞两个分支`master`和`hexo_source`分支，分别作为`github_pages`分支以及`hexo`博客源码分支。
这里由于github_pages好像只能展示master分支,所以源码分支就是非master分支了，但是可以把源码分支设置为主分支，每次写完博客，使用hexo deploy可以把博客静态网页自动推到`master`分支，然后再上传源码至`hexo_source`分支：
```
git add -u
git commit -m "xxx"
git push origin hexo_source
```
这样就完成了博客发布和整个博客源码的同步。

如果在新的设备上更新博客的话，
```
//拉取博客repo
git clone xxx.git
cd xxx

//安装hexo博客必要的软件


//愉快码字


//博客发布及源码同步
```