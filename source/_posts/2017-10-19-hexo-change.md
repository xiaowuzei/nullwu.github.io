---
title: hexo-change
date: 2017-10-19 08:26:34
tags: [hexo]
categories: 备忘录
---
## 将原先配置好并生成的hexo目录拷贝到新的电脑，只需拷贝以下目录：
```
 _config.yml
 package.json
 scaffolds/
 source/
 themes/
```
将这些目录放到一个目录下，如：hexo／
## 安装hexo
```
npm install -g hexo
```
## 安装好之后，进入hexo/目录
## 模块安装
```
 npm install
 npm install hexo-deployer-git --save
 npm install hexo-generator-feed --save
 npm install hexo-generator-sitemap --save
```
## 部署
```
hexo g
hexo deploy
```