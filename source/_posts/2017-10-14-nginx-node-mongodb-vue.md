---
title: linux下部署node项目
date: 2017-10-14 11:18:00
tags: [linux,node.js]
categories: 备忘录
---
## 前言
没有

## 部署node环境
想要运行node，自然要在服务器上部署node环境

阿里云上有篇教程讲解[https://help.aliyun.com/document_detail/50775.html?spm=a2c4g.11186623.6.835.57aa2529X1NvZA](部署Node.js项目（CentOS)

如果根据以上教程成功搞定，node环境已经安装完成，npm包管理器也有了。由于npm有些资源被屏蔽或者是国外资源的原因，经常会导致用npm安装依赖包的时候失败，所有我还需要npm的国内镜像---cnpm。

xshell 敲入命令
```
npm install -g cnpm --registry=http://registry.npm.taobao.org 

```
完成之后，我们就可以用cnpm代替npm来安装依赖包了。

> 另外，如果是用nvm来管理node，需要每次登陆服务器首先nvm use [node版本号]才能使用node和npm命令

![image](/images/ndoe01.png)
<!-- more -->

## 上传代码到服务器
将node代码上传到服务器的/blog(自己指定)目录下，解构如下

![image](/images/node02.png)

其中app.js中指定监听的端口

```javascript
app.set('port', process.env.PORT || 3389);
app.listen(app.get('port'), function() {
  console.log('Express server listening on port ' + app.get('port'));
});

```

利用xshell在此目录下运行

`node app.js`
即可启动此项目，前提3389这个接口是开放的

但是如果用这种方法，我们不能退出，因此要用`pm2`来管理我们的进程

## pm2管理进程
全局安装pm2

`cnpm install pm2 -g`

运行

`pm2 start app.js`

![image](/images/node03.png)

停止

`pm2 stop app.js`
