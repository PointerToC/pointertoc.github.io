---
title: hexo+github.pages搭建博客记录
date: 2024-12-08 21:20:34
tags:
---

记录第一次搭建博客中踩过的坑
<!-- more -->

##### 首语
 最开始想使用的是jekyll + github pages，从最开始搭建jekyll的本地测试环境开始就有点不想用jekyll了，感觉比较麻烦，换个主题各种gem包版本的不兼容。我需要的是一种傻瓜式的toolchain，毕竟我搭建博客的初衷不是折腾博客，而是记录和总结自己学习的一些知识。最后，看了写**hexo** 官方文档，自己试了下，半个小时不到完成本地测试预览+主题切换+部署到github.io。

##### hexo建站
我用的archlinux，首先安装nodejs
```
sudo pacman -S nodejs
```
很不幸，404了，替换了源也没用，在这种情况下可以去archlinux的官网下载对应的[安装包](https://archlinux.org/packages/extra/x86_64/nodejs/)。然后在本地手动安装
```
sudo pacman -U <package-name>
```
安装hexo包
```
npm install -g hexo-cli
```
创建博客目录，并初始化
```
mkdir <blog-folder>  
cd <blog-folder>
hexo init
```
更换主题，这里以更换[ZenMind](https://github.com/LenChou95/hexo-theme-ZenMind)主题为例。
```
cd <blog-folder>
git clone git@github.com:LenChou95/hexo-theme-ZenMind.git themes/ZenMind
```
打开站点配置文件`_config.yml`，将`theme`配置成`ZenMind`
```
theme: ZenMind
```
这个时候在本地敲`hexo g && hexo s`就能预览到更换的主题了。接下来便是部署。github创建guthub.io的repo过程不多介绍了。这里强调下`<blog-folder>`中的文件可以理解为**源文件**，通过`hexo g`命令生成的为**发布文件**，所谓的部署就是要将**发布文件**push到github的repo中对应的**发布分支**下，这个分支就是别人访问你的博客，能看到的内容。应此将repo中**settings->pages->source**的设置改为**Deploy from a branch**，branch可以设置为`main`。不是Hexo官网中说的，将该设置置为**GitHub Actions**（之前跟着hexo文档上的步骤走，遇到问题，后面改成这个方法后就没遇到过了）。
首先安装部署插件`hexo-deploy-git`
```
npm install hexo-deployer-git --save
```
编辑站点配置文件`_config.yml`
```
deploy:
    type: git 
    repository: git@github.com:usrname/usrname.github.io.git
    branch: main
```
这个`main`分支就是上述说的**发布分支**。然后部署
```
hexo d
```
这个时候访问你的博客就能看到内容了。
