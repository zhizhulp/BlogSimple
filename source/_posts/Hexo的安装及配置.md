---
title: Hexo在Windows下的安装及配置
date: 2019-05-31 14:37:05
tags: [Hexo]
categories: [web]
---


##### 安装Git

下载[git-for-windows](https://gitforwindows.org/) ，然后一步一步安装。

##### 安装Node

下载[node](https://nodejs.org/en/download/) ，然后一步一步安装。

##### 安装Hexo

``` bash
npm install hexo-cli -g 
```


##### Hexo的初始化

``` bash
hexo init blog
cd blog
npm install
```
##### Hexo的本地测试
``` bash
hexo s
```
##### Hexo部署
首先在你的github新建一个仓库，名字必须为仓库名.githun.io。
打开blog根目录下的_config.yml文件，加入或修改成以下配置

```
deploy:
type: git
repo: git@github.com:仓库名/仓库名.github.io.git
branch: master
```