---
title: linux下mongodb的安装配置
date: 2017-10-14 09:28:18
tags: [mongodb,linux]
categories: 备忘录
---
## 前言
本文将总结如何在linux系统下安装并运行mongb
服务器操作系统:CentOS 6.8 64位
<!-- more -->
## 下载上传解压三部曲
首先去[官网](http://note.youdao.com/)将mongodb下载到自己的本地(linux版本)
通过winScp上传到服务器任意目录
打开xshell到此目录下，敲入以下命令进行解压
```
tar xvf mongodb-linux-x86_64-amazon-3.4.9.tgz
```
将解压后的文件重命名并移动到/user/local目录下 
```
mv mongodb-linux-x86_64-amazon-3.4.9 /usr/local/mongodb
```
```
cd  /usr/local/mongodb/bin/
```
```
ll
```
![image](/images/mongodb01.png)
> bin下的mongod就是MongoDB的服务端进程，mongo就是其客户端，其它的命令用于MongoDB的其它用途如MongoDB文件导出等

## 启动
启动前，先指定mongodb的data目录，如果没有就创建一个：
```
cd /usr/local/mongodb
```
```
mkdir data
```
```
/usr/local/mongodb/bin/mongod --dbpath=/usr/local/mongodb/data/ --logpath=/usr/local/mongodb/data/mongodb.log --logappend&

```

启动成功后，可查看是否启动成功了，默认端口号是27017，当然在启动时也可以指定未使用的其它端口.
```
netstat -nutlp
```
![image](/images/mongodb02.png)

最后，将客户端mogo文件在/bin下软链接，方便随处执行：

```
ln -s /usr/local/mongodb/bin/mongo /bin/mongo
```
（完蛋）